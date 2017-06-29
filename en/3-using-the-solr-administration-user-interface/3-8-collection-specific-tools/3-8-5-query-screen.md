# 3.8.5. Query Screen

You can use the Query screen to submit a search query to a Solr collection and analyze the results.

In the example in the screen shot, a query has been submitted, and the screen shows the query results sent to the browser as JSON.

*Figure 1. JSON Results of a Query*

In this example, a query for `genre:fantasy` was sent to a "films" collection. Defaults were used for all other options in the form, which are explained briefly in the table below, and covered in detail in later parts of this Guide.

The response is shown to the right of the form. Requests to Solr are simply HTTP requests, and the query submitted is shown in light type above the results; if you click on this it will open a new browser window with just this reqest and response(without the rest of the Solr Admin UI). The rest of the response is shown in JSON, which is part of the request(see the wt=json part at the end).

The response has at least two sections, but may have several mmore depending on the options chosen. The two sections it always has are the `responseHeader` and the `response`. The `responseHeander` includes the status of the search(`status`), the processing time(QTime), and the parameters(params) that were used to process the query.

The response includes the documents that matched the query, in doc sub-sections. The fields return depend on the parmeters of the query (and the defaults of the request handler used). The number of results is also included  in this section.

This screen allows you to experiment with different query options, and inspect how your documents were indexed. The query parameters available on the form are some basic options that most users want to have available, but there are dozens more available which could be simple added to the basic request by hand(if opened in a browser). The table below explains the parameters available:

|Field|Description|
|---|---|
|Request-handler(qt)|Specifies the query handler for the request. If a query handler is not specified, Solr processes the response with the standard query handler.|
|q|The query event, See Searching for an explanation of this parameter.|
|fq|The filter queries. See Common Query Parameters for more information on this parameter.|
|sort|Sorts the response to a query in either ascending or descending order based on the response's score or another specified characteristic.|
|start, rows|`start` is the offset into the query result starting at which documents should be returned. The default value is 0, meaning that the query should return results starting with the first document that matches. This field accepts the same syntax as the start query parmeter, which is described in Searching. `rows` is the number of rows to return.|
|fl|Defines the fields to return for each document. You can explicitly list the stored fields, functions, and doc transformers you want to have returned by separating them with either a comma or a space.|
|wt|Specifies the Response Writer to be used to format the query response. Defaults to XML if not specified|
|indent|Click this button to request that Response Writer use indentation to make the responses more readable.|
|debugQuery|Click this buttion to augment the query response with debugging information, including "explain info" for each document returned. This debugging information is intended to be intelligible to the administrator or programmer.|
|dismax|Click this button to enable the Dismax query parser. See The Dismax query parser for further information|
|edismax|Click this button to enable the extended query parser. See the extended Dismax query parser for further information|
|hl|Click this button to enable highlighting in the query response. See Highlighting for more information.|
|facet|enables faceting, the arrangement of search results into categories based on indexed terms. See Faceting for more information.|
|spatial|Click to enable using location data for use in spatial or geospatial searches. See Spatial search for more information.|
|spellcheck|Click this button to enale the Spellchecker, which provides inline query suggestions based on other, similar, terms. See spell checking for more information|
