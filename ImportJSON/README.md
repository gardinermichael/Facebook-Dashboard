# ImportJSON

Import JSON from any URL directly into your Google Sheets. `ImportJSON.gs` adds an `=ImportJSON()` function to your spreadsheet, allowing quick and easy JSON importing. To use go to `Tools` > `Script Editor` and add the `ImportJSON.gs` file. Now in your spreadsheet you can access the `ImportJSON()` function. Use it like this:

    =ImportJSON("https://mysafeinfo.com/api/data?list=bestnovels&format=json&rows=20&alias=cnt=count,avg=average_rank,tt=title,au=author,yr=year", "/title")

Here are all the functions available:

| Function                |  Description                                                                      |
|-------------------------|-----------------------------------------------------------------------------------|
| **ImportJSON**          | For use by end users to import a JSON feed from a URL                             |
| **ImportJSONFromSheet** | For use by end users to import JSON from one of the Sheets                        |
| **ImportJSONViaPost**   | For use by end users to import a JSON feed from a URL using POST parameters       |
| **ImportJSONBasicAuth** | For use by end users to import a JSON feed from a URL with HTTP Basic Auth        |
| **ImportJSONAdvanced**  | For use by script developers to easily extend the functionality of this library   |

Review `ImportJSON.gs` for more info on how to use these in detail.
