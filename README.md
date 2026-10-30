# Jaspersoft Server Users & Roles Extractor

A small desktop tool that extracts the users and roles from a JasperReports
Server and writes them to an Excel file, so that access rights can be reviewed
without clicking through the server's admin UI.

Built in 2018 in Java with a JavaFX interface.

## What it does

1. Asks for the server URL, organisation, username and password.
2. Logs in through the server's REST API.
3. Reads all users and roles from the `rest_v2/users` and `rest_v2/roles`
   endpoints, and the server version and build from `rest_v2/serverInfo`.
4. Generates `Extracted_JS_users.xlsx` (Apache POI) with one sheet,
   `JS_Users`: one row per user (username and name) and one column per role,
   with a `*` where the user has that role. This makes it easy to review who
   has which access.

The window also reports the number of users and roles found and how long the
extraction took.

## Project structure

```
src/jsUsersRolesExtractor/
  Launcher.java               JavaFX window and entry point
  JSUsersRolesExtractor.java  REST calls, XML parsing and Excel generation
  JSUser.java                 User model (username, name, roles)
  aboutWindow.java            About dialog
  confirmWindow.java          Confirmation dialog
  OpenExcelFileException.java Raised when the output file is already open
```

## Build and run

Requires Java 8 with JavaFX. The project was developed in Eclipse and depends
on these libraries, which you need to add to the classpath:

- Apache POI 4.0 (poi, poi-ooxml and their dependencies)
- Apache Commons Collections 4.2
- Apache Commons HttpClient 3.x
- Log4j

Run `Launcher` to start the tool. The `.classpath` file contains absolute paths
from the original development machine. Update them to your own library
locations.

## Notes

- The tool needs an account with permission to read users and roles on the
  Jaspersoft server (typically an administrator).
- Credentials are sent in the login request URL, as the server's legacy login
  endpoint expects. Use it only against servers you trust, over HTTPS.
- Tested on the Jaspersoft server version used at the time. Newer server
  versions may change the REST responses.
