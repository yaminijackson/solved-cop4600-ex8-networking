Download Link: https://assignmentchef.com/product/solved-cop4600-ex8-networking
<br>
<h1>Overview</h1>
This exercise serves as a brief introduction to TCP servers and the network toolset. In this activity, you will create a “wall” application, in which a single user can connect in order to “tag” (leave a message on) the wall. Wall programs were common in the days of Bulletin Board Systems (BBSs) before other Internet services became popular. As connections were made via telephone hardline dial-up, only one user could connect at a time, so the “wall” program served as a community board:

As text mode screens are generally 25 rows and 80 columns, a limited number of wall messages can be held; when the wall is “full”, the oldest message is removed from the message queue, and the new message is added to the end (first-in-first-out).The server and client must communicate through a TCP socket (in <strong>C</strong> or <strong>C++</strong>) and <strong>must compile and run on Reptilian</strong>. The wall’s state should be maintained <u>even between connections</u>. The <strong>netcat</strong> command line utility should be used to test the server.
<h1>Specification</h1>
Your server will run as a standalone program from the command line and will use the protocol specified below.
<h2>Command Line Execution</h2>
The server program will take up to two parameters, optionally – the maximum number of messages stored and the port. If not provided, the port should default to 5514, while the number of messages should default to 20:

<strong>$</strong> <strong>./wallserver</strong> Queue size 20, Port 5514

<strong>$</strong> <strong>./wallserver 30 </strong> Queue size 30, Port 5514

<strong>$</strong> <strong>./wallserver 35 7777 </strong> Queue size 35, Port 7777
<h2>Server Behavior</h2>
When a client connects, the server should send the wall’s contents and a prompt as shown below (<strong>Figure 2a</strong>). If there are no message entries, it should instead send “<strong>[NO MESSAGES – WALL EMPTY]</strong>” (<strong>Figure 2b</strong>).
<table width="620">
<tbody>
<tr>
<td width="290">Wall Contents————-Ted: Iron Maiden?Bill: Excellent!Liz: Look! I am a human doing human things! Enter command: <strong>_</strong></td>
<td rowspan="2" width="40"></td>
<td width="290"></td>
</tr>
<tr>
<td width="290"><strong>Figure 2a. Wall display with contents. </strong></td>
<td width="290"><strong>Figure 2b. Wall display without contents. </strong></td>
</tr>
</tbody>
</table>
The server will accept four distinct commands – <strong>clear</strong>, <strong>post</strong>, <strong>kill</strong>, and <strong>quit</strong>. For commands that do not cause the user to disconnect (<strong>kill</strong> and <strong>quit</strong>), the server should send the wall’s contents and prompt the user for an additional command as shown in the example output.
<table width="267">
<tbody>
<tr>
<td width="267">Enter command: <strong>clear</strong>↵Wall cleared.Wall Contents————-[NO MESSAGES – WALL EMPTY]</td>
</tr>
</tbody>
</table>
<u>clear</u>

Clears the wall of all entries on the wall. In addition, the server should send a message indicating that the wall has been cleared. In addition, the server should send the wall’s contents to verify that the wall has in fact been cleared out correctly.
<table width="265">
<tbody>
<tr>
<td width="265">Wall Contents————-Jimmy Dean: Try my breakfast delights!Johnny 5: I’m alive!! Enter command: <strong>post</strong>↵Enter name: <strong>Bobo</strong>↵Post [Max length 74]: <strong>Hullo.</strong>↵Successfully tagged the wall. Wall Contents————-Johnny 5: I’m alive!!Bobo: Hullo. Enter command: <strong>_</strong></td>
</tr>
</tbody>
</table>
<u>post</u>

Indicates the user wishes to tag the wall. The user should be prompted for their name, followed by a message. The entire post should not exceed 80 characters (including name and separator), so the maximum length of the message should be indicated to the users. If the message is too long, the server should display the message “<strong>Error: message is too long!</strong>”; otherwise, it should display “<strong>Successfully tagged the wall.</strong>”

If the wall is “full” (the number of messages stored has reached the maximum), the oldest message (at the top) should be removed from the wall to make room for the new message post. Following the post attempt, the wall’s contents should be displayed and the user prompted for the next command. An example (with queue size 2) is shown on the right.
<table width="694">
<tbody>
<tr>
<td width="254">Wall Contents————-Jimmy Dean: Try my breakfast delights!Enter command: <strong>post</strong>↵Enter name: <strong>Johnny 5</strong>↵Post [Max length 70]: <strong>I’m alive!!!</strong> ↵ Successfully tagged the wall.</td>
<td width="14"></td>
<td width="426">Wall Contents————-Jimmy Dean: Try my breakfast delights!Enter command: <strong>post</strong>↵Enter name: <strong>~~~[[[[[[[[[[[THE PLAGUE]]]]]]]]]]]~~~</strong>↵Post [Max length 40]: <strong>12345678901234567890123456789012345678901</strong>↵Error: message is too long!</td>
</tr>
</tbody>
</table>
<table width="288">
<tbody>
<tr>
<td width="288">Enter command: <strong>kill</strong>↵Closing socket and terminating server. Bye!<strong>$ </strong><strong>_</strong></td>
</tr>
</tbody>
</table>
<u>kill</u>

Causes the server to shut down (terminate), and close the socket, disconnecting the user.
<table width="288">
<tbody>
<tr>
<td width="288">Enter command: <strong>quit</strong>↵ Come back soon. Bye! <strong>$ </strong><strong>_</strong></td>
</tr>
</tbody>
</table>
<u>quit</u>

Displays a termination message and closes the client’s socket, but <u>does not shut down the server or clear the wall</u>.
<h2>Debugging</h2>
It is recommended that students debug their server using the <strong>netcat</strong> command line utility (with alias <strong>nc</strong>). To do so, run your server in one ssh session, then open another and run <strong>netcat</strong>:
<table width="707">
<tbody>
<tr>
<td width="340"><strong>$ </strong> <strong>./wallserver 2</strong>↵ Wall server running on port 5514 with queue size 2.Students are recommended to</td>
<td width="15"></td>
<td width="352"><strong>$ </strong><strong>netcat localhost 5514</strong> ↵Wall Contents————-Pigeon: You’ve got mail. Err, I mean, cooo. Coooo. Enter command: <strong>quit</strong>↵ Come back soon. Bye! <strong>$ </strong><strong>_</strong></td>
</tr>
</tbody>
</table>
<h1>Submissions</h1>
You will submit the following at the end of this exercise:
<ul>
 	<li>Compressed tar archive (<strong>tar.gz</strong>) for the server and its Makefile</li>
 	<li>Screenshot of <strong>netcat</strong> connecting to the server to show its function <em>in detail</em> (described below)</li>
</ul>
<h2>Compressed Archive (ex8.tar.gz)</h2>
Your compressed tar file should have the following directory/file structure:

To build the server program and run it, we will execute these commands:

<strong>$ </strong><strong>tar zxvf ex8.tar.gz</strong> <strong>$ </strong><strong>cd ex8</strong> <strong>$ </strong><strong>make</strong> <strong>$ </strong><strong>./wallserver [lines] [port] </strong>

&nbsp;
<h2>Screenshot</h2>
Run the server with a message queue size of 5 and connect via <strong>netcat</strong>, running the following commands: 1) Post two messages successfully and one that fails due to being too long
<ul>
 	<li>Clear the server’s wall</li>
 	<li>Post to the wall again</li>
 	<li>Quit from the server</li>
 	<li>Reconnect to the server</li>
 	<li>Kill the server</li>
 	<li>Attempt to connect again to show that the server has terminated</li>
</ul>
You may use two screenshots if you cannot fit all the commands with a single capture.
<h1>Resources</h1>
<a href="https://linux.die.net/man/2/socket">https://linux.die.net/man/2/socket</a> – documentation for a function that you’ll be getting comfortable with <a href="https://www.unixfu.ch/use-netcat-instead-of-telnet/">https://www.unixfu.ch/use</a><a href="https://www.unixfu.ch/use-netcat-instead-of-telnet/">–</a><a href="https://www.unixfu.ch/use-netcat-instead-of-telnet/">netcat</a><a href="https://www.unixfu.ch/use-netcat-instead-of-telnet/">–</a><a href="https://www.unixfu.ch/use-netcat-instead-of-telnet/">instead</a><a href="https://www.unixfu.ch/use-netcat-instead-of-telnet/">–</a><a href="https://www.unixfu.ch/use-netcat-instead-of-telnet/">of</a><a href="https://www.unixfu.ch/use-netcat-instead-of-telnet/">–</a><a href="https://www.unixfu.ch/use-netcat-instead-of-telnet/">telnet/</a> – an http example using <strong>netcat</strong>

<a href="https://linuxhandbook.com/jobs-command/">https://linuxhandbook.com/jobs</a><a href="https://linuxhandbook.com/jobs-command/">–</a><a href="https://linuxhandbook.com/jobs-command/">command/</a> – as an alternative to opening multiple terminals, you can keep the server running in the background by learning how to manage jobs (not necessary to complete the exercise, but may be interesting to students that like working in the terminal)