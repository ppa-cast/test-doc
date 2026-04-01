"# test-doc" 
Application_Structure_Resources_2.12.md

Health_Results_Resources_2.12.md
Quality_and_Sizing_Model_Resources_2.12.md
Report Services_2.12.md
Server Services_2.12.md
User Session Services_2.11.md

--

Each markdown file has been converted to markdown from a HTML page edited with Confluence.

Some styling and formatting rules.

Each Web Service is described following a template: URI templates, parameters, JSON Examples etc.
Please apply the same template to insert new Web Services documentation or update existing web Services documentation.

The preferred approach is to format the descriptions (URI templates, parameters) with a table. You may need to insert `<br/>` separator to force a line break.

> IMPORTANT! However, when the content of the array is too complex, or when the columns are too much narrowed (because of word-break issues), then you can transform the table into a list of paragraphs with the following template:

    ## Quality Standards

    ### URI Templates & Parameters

    - **GET** `{Domain}/quality-standards-categories/{QualityStandardCategory}?{parameters}` 

      - *Description*:

        Array of all quality standard references (aka tags) for a given category (OWASP-2017, STIG-V4R8-CAT1, etc.)
        
      - *Media Type*:
        - `application/json`

    - **GET** `{Domain}/applications/{ID}/quality-standards` 

      - *Description*:

        Array of quality standard references for an application. Only quality standard references with violations are reported.
        
      - *Media Type*:
        - `application/json`

    - **GET** `{Domain}/applications/{ID}/snapshots/{SnapshotID}/quality-standards` 

      - *Description*:

        Quality standard references for an application snapshot
        
      - *Media Type*:
        - `application/json`

> Please check the rendering.

> Note that you can check the rendering before commit with an online markdown editor such as:
[https://jbt.github.io/markdown-editor/](https://jbt.github.io/markdown-editor/)