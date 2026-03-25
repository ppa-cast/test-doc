# Report Service - 2.11

*Created by N Padmavathi on Jan 31, 2023*

---

## On this page

- [Report](#report)
  - [Configuration](#configuration)
  - [URI Templates](#uri-templates)
  - [Error Messages](#error-messages)
  - [Example of use](#example-of-use)
  - [Administration Task](#administration-task)

---

## Report

This web service allows getting a report using the Report Generator in an **asynchronous** way.

The REST API spawns the Report Generator processes, and the REST API is also called back by the Report Generator itself (which runs as a REST API client).

> See the `server` resource object and the `reportEnabled` attribute to check whether this web service is enabled.

### Configuration

The `## REPORT CONFIGURATION` section in `application.properties` configures the interaction between the REST API and the Report Generator:

```properties
# Location of the Report Generator command line. If not set, the web services are not enabled.
report.reportGenerator=dotnet c:\\ReportGenerator\\CastReporting.Console.Core.dll

# Directory to save reports. Should be cleaned up periodically.
report.directory=c:\\temp\\reports

# URL to call back the REST API (localhost can be used since REST API and Report Generator share the same host).
# eg. http://localhost:8080/CAST-RESTAPI/rest  (WAR archive)
# eg. http://localhost:8080/rest               (ZIP archive)
report.webServiceURL=http://localhost:8080/CAST-RESTAPI/rest

# Maximum number of concurrent Report Generator processes.
report.maxConcurrentProcesses=4

# Delay before killing a never-ending Report Generator process (in seconds).
report.maxDelayInSeconds=600
```

### URI Templates

| HTTP Action | Media Type | URI Templates | Description |
|---|---|---|---|
| GET | `*/*` | `{Domain}/reports?{parameters}` | Get a report as an "open office" document file. Returns: `202 Accepted` (in progress), `200 OK` (ready, file downloaded), `503 Service Unavailable` (disabled/misconfigured), `500 Internal Server Error` (generation failed) |

**Parameters**

| URI Parameter | Description | Values | Default value |
|---|---|---|---|
| template | A Report Generator template name | String | N/A |
| snapshot-id | A snapshot ID to identify an application snapshot | Integer | N/A |
| previous-snapshot-id | A previous snapshot ID (if required by the template) | Integer | N/A |

### Error Messages

| HTTP Status | Response | Remediation |
|---|---|---|
| 503 Service Unavailable | `"cause": "The report generator is not enabled for the REST API"` | Install the Report Generator and set `report.reportGenerator` path |
| 503 Service Unavailable | `"cause": "Cannot run Report Generator. Check report.reportGenerator setting"` | Incorrect path — check and fix `report.reportGenerator` |
| 503 Service Unavailable | `"cause": "Cannot access reports directory. Check report.directory setting"` | Incorrect path or missing permissions — check `report.directory` |
| 503 Service Unavailable | `"cause": "Report Generator error (exit code: 1) - Bad command line"` | Install the latest version of the REST API |
| 503 Service Unavailable | `"cause": "Report Generator error (exit code: 2) - Cannot access to the REST API. Check report.webServiceURL setting"` | Check `report.webServiceURL` with a browser or curl |
| 500 Internal Error | `"cause": "Report Generator error (exit code: 3) - Internal error. See the log files"` | Check Report Generator log files or contact CAST Support |
| 500 Internal Error | `"cause": "Report Generator processing has exceeded 600 seconds."` | Increase `report.maxDelayInSeconds` or investigate the Report Generator |
| 500 Internal Error | `"message": "Unexpected end of generation. The document was not found"` | Check that the Report Generator can write to the reports directory |

### Example of use

```bash
#################
# Check whether the report generator is enabled
#################
curl -u admin:cast http://localhost:8080/CAST-RESTAPI/rest/server
# {"href":"server","name":"Server",... "reportEnabled":true, ...}

#################
# Request for a report -> 202 Accepted (not yet ready)
#################
curl -u admin:cast \
  "http://localhost:8080/CAST-RESTAPI/rest/WEBGOAT/reports/?template=CISQ+-+Top+22+-+Summary.docx&snapshot-id=1" \
  -O -J -v
# < HTTP/1.1 202 Accepted

#################
# Request for a report -> 200 OK (ready, downloaded)
#################
curl -u admin:cast \
  "http://localhost:8080/CAST-RESTAPI/rest/WEBGOAT/reports/?template=CISQ+-+Top+22+-+Summary.docx&snapshot-id=1" \
  -O -J -v
# < HTTP/1.1 200 OK
# < Content-Disposition: attachment; filename="WEBGOAT-1-CISQ - Top 22 - Summary.docx"
# < Content-Type: application/vnd.openxmlformats-officedocument.wordprocessingml.document
# curl: Saved to filename 'WEBGOAT-1-CISQ - Top 22 - Summary.docx'
```

### Administration Task

The reports directory is **never cleaned up automatically**.

The site administrator must ensure that enough disk space is available and set up a periodic cleanup (e.g., a cron script based on file dates).
