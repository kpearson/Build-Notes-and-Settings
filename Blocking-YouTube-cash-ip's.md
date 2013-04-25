#Reject certain IP addresses asosiated with youtube cashing.
http://mitchribar.com/2013/02/time-warner-cable-sucks-for-youtube-twitchtv/

Doing this will cause the stream to take 1-2 seconds to start because of the IP rejection handling, but then you are greeted with a silky smooth, ultra fast experience.

###To reject certain IP addresses and get back to blazingly fast Youtube streaming (and Twitch.tv). 

Open up your terminal and run these commands (you will be prompted for the admin password):

<pre><code>sudo ipfw add reject src-ip 173.194.55.0/24 in</code></pre>
<pre><code>sudo ipfw add reject src-ip 206.111.0.0/16 in</code></pre>

You can check the rules were added by using this command:

<pre><code>sudo ipfw list</code></pre>

Need to delete these rules? No problem, just run the ‘sudo ipfw list’ command again. Next to each rule is a reference number, so take the number of the rule you want to delete and replace the ‘X’ in this command with that number:

<pre><code>sudo ipfw delete X</code></pre>

These commands create a better streaming experience because, TWC is throttling downloads from servers (CDN) that host cached videos. By rejecting these IP address ranges you will force the video to be served to you directly. This harnesses the full download speed of your internet connection. Other people can dive into the complexity much better than I ever could, but that’s the overall theme.

