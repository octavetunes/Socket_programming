# Socket Programming - File Transfer Simulation with Integrity Check
The application respects the following protocol:

    Handshake Phase:

        The server awaits for a new connection.

        The client connects to the server.

        The server expects one of the following commands from the client:

            LIST FILES: The client sends the “LIST FILE” command in ASCII and awaits from the server a reply with a list of files that are available for download. The server’s reply format is “file1 id;file1 name;file1 size (newline) file2 id;file2 name;file2 size (newline) ...” or a custom message in case there are no files (for example: “No files available at the moment”)

            DOWNLOAD: The client sends the “DOWNLOAD” command in ASCII. The server will reply, asking for a “file id” of the file that should be downloaded to the client. Then, the client sends the “file id” and awaits for the file as byte-stream.

            UPLOAD: The client sends the “UPLOAD” command in ASCII. The server will reply, asking for the “File Name” and “File Size”. The client sends the file details in the following format “file_name;file size” in ASCII. Once the server receives the file details, it will reply that it’s ready to receive the file as byte-stream. Only after this reply the client will start sending the file as byte-stream.

    Verification Phase:

    To check whether an uploaded or downloaded file was transferred intact, a verification is required. To achieve this, both the server and client each generate an MD5 hash of the file. When uploading, the server will send its generated hash to the client for comparison. When downloading, you can utilize the “file id” as the MD5 hash.

Simulation order is as follows:

    Start the server (listens to clients).
    Start the client.
    The client connects to the server.
    The client sends a 'LIST_FILES' command.
    The server will reply with a message that there are no files at the moment.
    The client then sends the 'UPLOAD' command.
    The server asks for the filename and filesize.
    The client sends the file details (for example: “my_file.jpg;123456”).
    The server sends a notification message (for example: “ready to receive a file”).
    The client starts sending the file.
    The server receives the file and generates the MD5 Hash (the hash will be used as file id).
    The server saves the received file (writes the received data into a file).
    The server sends the MD5 hash to the client.
    The client generates his own MD5 hash of the file and compares it to the one received from the server. In case of a match the client will print a “Success” message, otherwise a “Fail” message will be printed.
    The client sends again a 'LIST_FILES' command.
    The server will reply with the available files (in this case: “file id;my_file.jpg;12345”, where file id is the MD5 hash of the file).
    The client sends a 'DOWNLOAD' command.
    The server asks for a file id (for example: “Please send a file ID”).
    The client sends the file id extracted from the previous LIST_FILES command.
    The server sends the file to the client.
    The client receives the file and generates the MD5 hash.
    The client disconnects from the server.
