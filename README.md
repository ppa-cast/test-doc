"# test-doc" 
Application_Structure_Resources_2.12.md

Health_Results_Resources_2.12.md
Quality_and_Sizing_Model_Resources_2.12.md
Report Services_2.12.md
Server Services_2.12.md
User Session Services_2.11.md

--

Each markdown file has been converted from a HTML page edited with Confluence
see [https://doc.castsoftware.com/export/DASHBOARDS/REST+API+Reference+Documentation+-+2.12](https://doc.castsoftware.com/export/DASHBOARDS/REST+API+Reference+Documentation+-+2.12)

### Some styling and formatting rules.

Each Web Service is described following a template: URI templates, URI parameters, JSON Examples etc.
Please apply the same template to insert new Web Services documentation or update existing web Services documentation.

The preferred approach is to format the descriptions (URI templates, URI parameters) tables. You may need to insert `<br/>` separator to force a line break.

> IMPORTANT! However, when the content of the array is too complex, or when the columns are too much narrowed (because of word-break issues), then you can transform the table into a list of paragraphs with the following template:

    ## Assessment Result

    ### URI Templates 

    - **GET** `{Domain}/results{?parameters}`

      - *Description*:

        Array of results of a domain split by snapshots, by applications

      - *Media Type*:
        - `application/json`
        - `text/csv`
        - `application/vnd.openxmlformats-officedocument.spreadsheetml.sheet`

    - **PUT** `{Domain}/results{?parameters}`

      - *Description*:

        Create or update background facts values in a AED or AAD domain for several applications in the last snapshot

      - *Media Type*:
        - `text/csv`

    ### URI Parameters

    - **quality-indicators**

        - *Description:* Specify a list of quality indicators

        - *Values:* Quality Indicators identifiers or keywords, separated by a comma

            - `.../results/?quality-indicators=(business-criteria)`
            - `.../results/?quality-indicators=(technical-criteria)`
            - `.../results/?quality-indicators=(quality-rules)`
            - `.../results/?quality-indicators=(quality-distributions)`
            - `.../results/?quality-indicators=(quality-measures)`

            Use `c:` modifier to specify direct grade contributors of a Quality Indicator or a Quality Standard reference (aka tag)

            - `c:61027`
            - `c:CISQ`

            > Note: this list is based on the union set of targeted snapshots

> Please check the rendering for each documentation update.

> Note that you can check the rendering before a 'commit' with a markdown editor such as:
[https://jbt.github.io/markdown-editor/](https://jbt.github.io/markdown-editor/)