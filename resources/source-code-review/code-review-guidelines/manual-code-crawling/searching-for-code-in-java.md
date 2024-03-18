# Searching for Code in Java

**Input and Output Streams**

These are used to read data into one’s application. They may be potential entry points into an application. The entry points may be from an external source and must be investigated. These may also be used in path traversal attacks or DoS attacks.

| **STRING TO SEARCH**     |                         |                    |                      |
| ------------------------ | ----------------------- | ------------------ | -------------------- |
| FileInputStream          | ObjectInputStream       | FilterInputStream  | PipedInputStream     |
| SequenceInputStream      | StringBufferInputStream | BufferedReader     | ByteArrayInputStream |
| java.io.FileOutputStream | File                    | ObjectInputStream  | PipedInputStream     |
| StreamTokenizer          | getResourceAsStream     | java.io.FileReader | java.io.FileWriter   |
| java.io.RandomAccessFile | java.io.File            | renameTo           | Mkdir                |

**Cross Site Scripting**

These API calls should be checked in code review as they could be a source of Cross Site Scripting vulnerabilities

| **STRING TO SEARCH**                    |        |   |   |
| --------------------------------------- | ------ | - | - |
| javax.servlet.ServletOutputStream.print | strcpy |   |   |

**Response Splitting**

Response splitting allows an attacker to take control of the response body by adding extra CRLFs into headers. In HTTP the headers and bodies are separated by 2 CRLF characters, and thus if an attackers input is used in a response header, and that input contained 2 CRLFs, then anything after the CRLFs would be interpreted as the response body. In code review ensure functionality is sanitizing any information being put into headers.

| **STRING TO SEARCH**                                |        |           |   |
| --------------------------------------------------- | ------ | --------- | - |
| javax.servlet.http.HttpServletResponse.sendRedirect | strcpy | setHeader |   |

**Servlets**

These API calls may be avenues for parameter/header/URL/cookie tampering, HTTP Response Splitting and information leakage. They should be examined closely as many of such APIs obtain the parameters directly from HTTP requests.

| **STRING TO SEARCH** |                       |                    |                           |
| -------------------- | --------------------- | ------------------ | ------------------------- |
| javax.servlet.\*     | getParameterNames     | getParameterValues | getParameter              |
| getParameterMap      | getScheme             | getProtocol        | getContentType            |
| getServerName        | getRemoteAddr         | getRemoteHost      | getRealPath               |
| getLocalName         | getAttribute          | getAttributeNames  | getLocalAddr              |
| getAuthType          | getRemoteUser         | getCookies         | isSecure                  |
| HttpServletRequest   | getQueryString        | getHeaderNames     | getHeaders                |
| getPrincipal         | getUserPrincipal      | isUserInRole       | getInputStream            |
| getOutputStream      | getWriter             | addCookie          | addHeader                 |
| setHeader            | setAttribute          | putValue           | javax.servlet.http.Cookie |
| getName              | getPath               | getDomain          | getComment                |
| getMethod            | getPath               | getReader          | getRealPath               |
| getRequestURI        | getRequestURL         | getServerName      | getValue                  |
| getValueNames        | getRequestedSessionId |                    |                           |

**Redirection**

Any time an application is sending a redirect response, ensure that the logic involved cannot be manipulated by an attackers input. Especially when input is used to determine where the redirect goes to.

| **STRING TO SEARCH** |           |           |          |
| -------------------- | --------- | --------- | -------- |
| sendRedirect         | setStatus | addHeader | etHeader |

**SQL & Database**

Searching for Java database related code should help pinpoint classes/methods which are involved in the persistence layer of the application being reviewed.

| **STRING TO SEARCH**                 |                              |              |                  |
| ------------------------------------ | ---------------------------- | ------------ | ---------------- |
| java.sql.Connection.prepareStatement | java.sql.ResultSet.getObject | select       | insert           |
| java.sql.Statement.executeUpdate     | java.sql.Statement.addBatch  | execute      | executestatement |
| createStatement                      | java.sql.ResultSet.getString | executeQuery | jdbc             |
| java.sql.Statement.executeQuery      | java.sql.Statement.execute   | delete       | update           |
| java.sql.Connection.prepareCall      |                              |              |                  |

**SSL**

Looking for code which utilizes SSL as a medium for point to point encryption. The following fragments should indicate where SSL functionality has been developed.

| **STRING TO SEARCH** |                   |                  |                     |
| -------------------- | ----------------- | ---------------- | ------------------- |
| com.sun.net.ssl      | SSLContext        | SSLSocketFactory | TrustManagerFactory |
| HttpsURLConnection   | KeyManagerFactory |                  |                     |

**Session Management**

The following APIs should be checked in code review when they control session management.

| **STRING TO SEARCH** |            |       |   |
| -------------------- | ---------- | ----- | - |
| getSession           | invalidate | getId |   |

**Legacy Interaction**

Here we may be vulnerable to command injection attacks or OS injection attacks. Java linking to the native OS can cause serious issues and potentially give rise to total server compromise.

| **STRING TO SEARCH**   |                              |       |   |
| ---------------------- | ---------------------------- | ----- | - |
| java.lang.Runtime.exec | java.lang.Runtime.getRuntime | getId |   |

**Logging**

We may come across some information leakage by examining code below contained in one’s application.

| **STRING TO SEARCH**      |       |          |            |
| ------------------------- | ----- | -------- | ---------- |
| java.io.PrintStream.write | log4j | jLo      | Lumberjack |
| MonoLog                   | qflog | just4log | log4Ant    |
| JDLabAgent                |       |          |            |

**Ajax and JavaScript**

Look for Ajax usage, and possible JavaScript issues:

| **STRING TO SEARCH** |              |                 |                 |
| -------------------- | ------------ | --------------- | --------------- |
| document.write       | eval         | document.cookie | window.location |
| document.URL         | document.URL |                 |                 |
