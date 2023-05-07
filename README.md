Download Link: https://assignmentchef.com/product/solved-networkprogramming-homework-3-non-blocking-file-transfer
<br>
In this homework, you have to practice Nonblocking I/O in a network program

Introduction

You should write a server and a client. The server has to be a single-process, single-thread, and non-blocking server, and all connections are TCP in this homework.

One user can login the server on different hosts at the same time. Each client belongs to the same user should have the same view of files on the server.

The scenario of this homework is similar to Dropbox:

<ul>

 <li>A user can upload and save his files on the server.</li>

 <li>The clients of the user are running on different hosts.</li>

 <li>When any of client of the user uploads a file, the server has to transmit it to all other clients.</li>

 <li>When a new client connects to the server, the server should transmit all the files, which have been uploaded by the other clients, to the new client immediately.</li>

 <li>We will type put &lt;filename&gt; on different clients at the same time, and your programs have to deal with this case.</li>

 <li>If one of the clients is sleeping, the server should send the file data to other clients in a non-blocking way.</li>

 <li>The uploading data only need to be sent to the clients that belong to the same user.</li>

</ul>

<strong>Note</strong>: In your server program, you can include the following two lines to achieve the non-blocking function.

int flag = fcntl(sock, F_GETFL, 0);fcnctl(sock, F_SETTFL, flag|O_NONBLOCK);

Please confirm that your server operates in the non-blocking mode. We will purposely let the socket send buffer full to test the correctness of your server.

<h2>Inputs</h2>

<ol>

 <li>./server &lt;port&gt;Please make sure that you execute the server program in this format.</li>

 <li>./client &lt;ip&gt; &lt;port&gt; &lt;username&gt;Please make sure that you execute the client program in this format.</li>

 <li>put &lt;filename&gt;This command, which is executed on the client side by the user, is to upload your files to the server side.Users can transmit any files they want. But these files, after being received by the server, need to be stored in the same directory created for the user on the server side.Each file should be sent to the other clients belonging to the same user.The file to be uploaded should reside in the same directory with the client program.</li>

 <li>sleep &lt;seconds&gt;This command is to let the client sleep for the specified period of time.</li>

 <li>exitThis command is to disconnect with the server and terminate the program.</li>

</ol>

<h2>Outputs</h2>

<ol>

 <li>Welcome message. (displayed at the client side)</li>

</ol>

Welcome to the dropbox-like server: &lt;username&gt;

<ol>

 <li>Uploading progess bar. (displayed at the sending client side)</li>

</ol>

[Upload] &lt;filename&gt; Start!Progress : [######################][Upload] &lt;filename&gt; Finish!

<ol>

 <li>Downing progess bar. (displayed at the receiving client side)</li>

</ol>

[Download] &lt;filename&gt; Start!Progress : [######################][Download] &lt;filename&gt; Finish!

<ol>

 <li>Sleeping count down. (displayed at the client side)</li>

</ol>

sleep 20The client starts to sleep.Sleep 1..Sleep 19Sleep 20Client wakes up.

<strong>Note</strong>: Your progress bar should have the following format.They are represented by the # signs and the whitespaces.The # signs mean how much data your client has uploaded/downloaded.The whitespaces mean how much data your client hasn’t uploaded/downloaded.The sum of the # signs and whitespaces is twenty.

Progress [###                 ] // three # signs mean that your download/upload progress has completed 15%.Progress [####################] // twenty # signs mean that your download/upload progress finishes.

Client should execute either one upload process or one download process in the same time.

<strong>Tip</strong>:You can use ‘/r’ to reset cursor to the beginning of the same line.

<h2>Test Cases</h2>

<h3>Steps</h3>

<ol>

 <li>First, make sure that two clients of userA have connected to the server.After one client uploads a test file to the server, the other should receive that test file from the server.</li>

 <li>When a new client of userA connects to the server (now there are totally three clients of userA on ther server), the server has to transmit the test file uploaded in step 1 to this new client.</li>

 <li>Two clients of userA upload a test file with different names respectively at the same time.At the end of file transmission, all clients of userA must have the same set of files. That is, the two sending clients should receive the file uploaded by each other, and the other client should receive both files.</li>

 <li>After a new client of userA connects to the server (now there are totally four clients of userA on ther server), we will execute sleep 20 on this client to force it to sleep for 20 seconds. Within this 20 seconds, we will let another client transmit a file to the server. After receiving the file, the server will transmit it to all the other clients of the user, including the <strong>“sleeping”</strong> Your server should continually send the file data to clients even when one of the client is sleeping.</li>

 <li>When a new client of userB connects to the server, suppose a client of userA upload a test file, the server should not send the test file to the client of userB.</li>

 <li>Exit all clients.</li>

</ol>

<strong>Note</strong>:We will use <strong>“diff”</strong> to compare the files sent and the file received to check whether they are the same to ensure that the file transfers are complete.You <strong>cannot</strong> use <strong>“fork”</strong> system call or <strong>“thread”</strong> function call in this homework. Your server must be a single-process, single-thread, and non-blocking server.

<strong>Note</strong>:

When a client receives a file from the server, it has to store the file in the current directory of the client.

current directory of client├── client├── testfile1├── testfile2└── ……

When a user first uploads a file to the server, the server will create a directory for the user which is named after the user’s name in the current directory of the server.

Then, your server should store the files which belong to the user in his directory.

current directory of server├── server└── tom     ├── testfile1     └── ……

<h3>Output</h3>

The following cases will be executed one by one.

<h4><strong>[Execute Server]</strong></h4>

[<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="b0c5c3d5c2f0d9dec081">[email protected]</a> ~/hw3 ] ./server 8888

<h4><strong>[Case 1: Upload a file] </strong></h4>

[<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="493c3a2c3b0920273978">[email protected]</a> ~/hw3 ] cp testfile 0/testfile  # In Terminal 0[<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="a6d3d5c3d4e6cfc8d697">[email protected]</a> ~/hw3/0 ] ./client 127.0.0.1 8888 tomWelcome to the dropbox-like server: tom # In Terminal 1[<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="b4c1c7d1c6f4dddac485">[email protected]</a> ~/hw3/1 ] ./client 127.0.0.1 8888 tomWelcome to the dropbox-like server: tom # In Terminal 0[<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="6d181e081f2d04031d5c">[email protected]</a> ~/hw3/0 ] put testfile[Upload] testfile Start!Progress : [######################][Upload] testfile Finish! # In Terminal 1[<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="413432243301282f3170">[email protected]</a> ~/hw3/1 ] [Download] testfile Start!Progress : [######################][Download] testfile Finish! [<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="6510160017250c0b1554">[email protected]</a> ~/hw3 ] diff 0/testfile 1/testfile

<h4><strong>[Case 2: A new client of the same user logs in] </strong># Terminal 2[<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="80f5f3e5f2c0e9eef0b1">[email protected]</a> ~/hw3/2 ] ./client 127.0.0.1 8888 tomWelcome to the dropbox-like server: tom[Download] testfile Start!Progress : [######################][Download] testfile Finish! [<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="7306001601331a1d0342">[email protected]</a> ~/hw3 ] diff 0/testfile 2/testfile</h4>

<h4><strong>[Case 3: Upload files from different clients at the same time] </strong></h4>

[<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="6712140215270e091756">[email protected]</a> ~/hw3 ] cp testfile2 0/testfile2[<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="0277716770426b6c7233">[email protected]</a> ~/hw3 ] cp testfile3 1/testfile3 # Terminal 0[<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="6d181e081f2d04031d5c">[email protected]</a> ~/hw3/0 ] put testfile2[Upload] testfile2 Start!Progress : [######################][Upload] testfile2 Finish! # Terminal 1[<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="0772746275476e697736">[email protected]</a> ~/hw3/1 ] put testfile3[Upload] testfile3 Start!Progress : [######################][Upload] testfile3 Finish! # Terminal 2[<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="cebbbdabbc8ea7a0beff">[email protected]</a> ~/hw3/2 ] [Download] testfile2 Start!Progress : [######################][Download] testfile2 Finish![Download] testfile3 Start!Progress : [######################][Download] testfile3 Finish! # Terminal 0[<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="5227213720123b3c2263">[email protected]</a> ~/hw3/0 ] [Download] testfile3 Start!Progress : [######################][Download] testfile3 Finish! # Terminal 1[<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="bfcaccdacdffd6d1cf">[email protected]</a>1 ~/hw3/1 ] [Download] testfile2 Start!Progress : [######################][Download] testfile2 Finish! [<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="790c0a1c0b3910170948">[email protected]</a> ~/hw3 ] diff 0/testfile2 1/testfile2[<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="4d383e283f0d24233d7c">[email protected]</a> ~/hw3 ] diff 0/testfile2 2/testfile2[<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="acd9dfc9deecc5c2dc9d">[email protected]</a> ~/hw3 ] diff 1/testfile3 2/testfile3[<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="a2d7d1c7d0e2cbccd293">[email protected]</a> ~/hw3 ] diff 1/testfile3 0/testfile3

<h4><strong>[Case 4: Put a client to sleep] </strong></h4>

[<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="0570766077456c6b7534">[email protected]</a> ~/hw3 ] cp testfile4 0/testfile4 # Terminal 3[<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="c4b1b7a1b684adaab4f5">[email protected]</a> ~/hw3/3 ] ./client 127.0.0.1 8888 tomWelcome to the dropbox-like server: tomsleep 20The client starts to sleep.Sleep 1..Sleep 19Sleep 20Client wakes up.  # Terminal 0[<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="0772746275476e697736">[email protected]</a> ~/hw3/0 ] put testfile4[Upload] testfile4 Start!Progress : [######################][Upload] testfile4 Finish! # Terminal 1[<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="c4b1b7a1b684adaab4f5">[email protected]</a> ~/hw3/1 ] [Download] testfile4 Start!Progress : [######################][Download] testfile4 Finish! # Terminal 2[<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="0f7a7c6a7d4f66617f3e">[email protected]</a> ~/hw3/2 ] [Download] testfile4 Start!Progress : [######################][Download] testfile4 Finish! # Terminal 3[<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="cbbeb8aeb98ba2a5bbfa">[email protected]</a> ~/hw3/3 ] [Download] testfile4 Start!Progress : [######################][Download] testfile4 Finish! [<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="fc898f998ebc95928ccd">[email protected]</a> ~/hw3 ] diff 0/testfile4 1/testfile4[<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="0471776176446d6a7435">[email protected]</a> ~/hw3 ] diff 0/testfile4 2/testfile4[<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="80f5f3e5f2c0e9eef0b1">[email protected]</a> ~/hw3 ] diff 0/testfile4 3/testfile4

<h4><strong>[Case 5: Separation of different users] </strong></h4>

[<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="2d585e485f6d44435d1c">[email protected]</a> ~/hw3 ] cp testfile5 0/testfile5# Terminal 4[<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="6f1a1c0a1d2f06011f5e">[email protected]</a> ~/hw3/4 ] ./client 127.0.0.1 8888 frankWelcome to the dropbox-like server: frank # Terminal 0[<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="304543554270595e4001">[email protected]</a> ~/hw3/0 ] put testfile5[Upload] testfile5 Start!Progress : [######################][Upload] testfile5 Finish!# Terminal 1[<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="7603051304361f180647">[email protected]</a> ~/hw3/1 ] [Download] testfile5 Start!Progress : [######################][Download] testfile5 Finish! # Terminal 2[<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="1663657364567f786627">[email protected]</a> ~/hw3/2 ] [Download] testfile5 Start!Progress : [######################][Download] testfile5 Finish! # Terminal 3[<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="e491978196a48d8a94d5">[email protected]</a> ~/hw3/3 ] [Download] testfile5 Start!Progress : [######################][Download] testfile5 Finish! [<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="e396908691a38a8d93d2">[email protected]</a> ~/hw3 ] diff 0/testfile5 1/testfile5[<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="5722243225173e392766">[email protected]</a> ~/hw3 ] diff 0/testfile5 2/testfile5[<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="4a3f392f380a23243a7b">[email protected]</a> ~/hw3 ] diff 0/testfile5 3/testfile5[<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="6316100611230a0d1352">[email protected]</a> ~/hw3 ] [ -e 4/testfile5 ]

<h4><strong>[Case 6: Exit] </strong></h4>

# Terminal 0[<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="cabfb9afb88aa3a4bafb">[email protected]</a> ~/hw3/0 ] exit# Terminal 1[<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="b1c4c2d4c3f1d8dfc180">[email protected]</a> ~/hw3/1 ] exit# Terminal 2[<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="6510160017250c0b1554">[email protected]</a> ~/hw3/2 ] exit# Terminal 3[<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="3742445245775e594706">[email protected]</a> ~/hw3/3 ] exit# Terminal 4[<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="b8cdcbddcaf8d1d6c889">[email protected]</a> ~/hw3/4 ] exit