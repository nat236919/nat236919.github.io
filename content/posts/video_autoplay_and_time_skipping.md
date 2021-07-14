---
title: "Video Autoplay and Time Skipping"
date: 2020-08-06T09:30:11+08:00
draft: false
categories: ["blog"]
tags: ["blog", "web browser", "javascript", "videojs"]
---

Recently, I have been tasked to maintain one of the projects that provide a video player service powered by a powerful library [videojs](https://videojs.com/) which claims to be the most popular open-source HTML5 player framework, according to its site.

{{< image src="https://i.ibb.co/cxkGmLb/video-js.png" alt="videojs" position="center">}}

The previous engineers working before me had contributed a tremendous work by creating a customised version of our own player which provides functionalities, such as time markers, etc. It worked perfectly. Until I was asked to implement three new functionalities onto the video player:

1. Auto time-marking
2. Autoplay
3. Time Skipping

## Auto time-marking

Here is where good documentation and clean code come into play the most. The task was to receive a start time and an end time for a video and mark those times onto the player. Thanks to the previous engineers whose code was clean and easy to read. I was able to re-use an old component which allows a user to manually mark a point on a video timeline.

Basically, what I did was modifying the concerned functions to handle an array consisting of a start time and end time. Then proceed to add a marker.

```javascript
function common_util_value_is_null(exp) {
  return typeof exp === "undefined" || exp === null || exp == undefined;
}

function layout_util_file_display_movie_set_time_marker(obj_player, i_time) {
  if (common_util_value_is_null(i_time)) return false;
  str_video_plugin_id = "video_plugin_name_example";
  obj_player[str_video_plugin_id].addMark(i_time, "general");
}
```

Unfortunately, this method created a huge problem. The problem was that if a given time exceeds a video's length, the player will be overflown and it messed up all the layout.

{{< image src="https://i.ibb.co/BwKbngk/marker-overflown.png" alt="marker overflown" position="center">}}

To solve this problem, we must check a video's length (duration), then proceed to add markers. However, a video's length cannot be checked without being played first; this had become an issue. After gliding through all documentation and StackOverflow posts.

I came across a solution, by using **loadedmetadata** will allow us to get its data beforehand which was what I needed. Combining with some conditions to assure that there would be no layout overflows, this is the result.

```javascript
obj_player.on("loadedmetadata", function () {
  if (
    !common_util_value_is_null(i_start_time) &&
    i_start_time >= 0 &&
    i_start_time <= obj_player.duration()
  ) {
    layout_util_file_display_movie_set_time_marker(obj_player, i_start_time);
  }
  if (
    !common_util_value_is_null(i_end_time) &&
    i_end_time > i_start_time &&
    i_end_time <= obj_player.duration()
  ) {
    layout_util_file_display_movie_set_time_marker(obj_player, i_end_time);
  }
});
```

{{< image src="https://i.ibb.co/4Fzd91F/marked-video.png" alt="marked video" position="center">}}

After some testing with different test cases. Time markers are presented correctly. Now moving on to the next task **autoplay**.

## Autoplay

This should have been an easy task given that **videojs** already provides a method namely **play()** to help us. By defining simple procedures, a small snippet code would do the job.

```javascript
$(document).ready(function () {
  obj_player.play();
});
```

Was I wrong, so wrong. Autoplay did not trigger at all on Chrome and Firefox, yet it worked on Safari and Edge. I had spent a great deal of time to figure out what went wrong. Eventually, it turned out that this problem had something to do with [Autoplay Policy Changes](https://developers.google.com/web/updates/2017/09/autoplay-policy-changes).

As the Internet browsing has been moving towards improving UX, reducing ads, or even saving data consumption. Videos that have unmuted audio are **NOT** allowed to autoplay. Therefore, unless manually allow on individual browsers or a user has interacted with an interface, autoplay will only be triggered once the audio has been muted. Eventually, I decided to drop this implementation and let users manually click to play videos.

## Time Skipping

The last implementation was that we would like to have a video started playing from a specific time. Once again, **videojs** provides a method called **currentTime()** to help us. Without waiting for too long, I used this method in the existing code.

```javascript
obj_player.on("loadedmetadata", function () {
  if (
    !common_util_value_is_null(i_start_time) &&
    i_start_time >= 0 &&
    i_start_time <= obj_player.duration()
  ) {
    layout_util_file_display_movie_set_time_marker(obj_player, i_start_time);
    obj_player.currentTime(i_start_time);
  }
  if (
    !common_util_value_is_null(i_end_time) &&
    i_end_time > i_start_time &&
    i_end_time <= obj_player.duration()
  ) {
    layout_util_file_display_movie_set_time_marker(obj_player, i_end_time);
  }
});
```

Succeeded!! It worked like a charm, videos were able to play from given start time. Case closed.

{{< image src="https://i.ibb.co/QCmS2Hs/marked-video-current-time.png" alt="marked video with currentTime" position="center">}}

Little did I know, it **DID NOT** work on **iOS Safari**. I found this out weeks later after receiving this bug report. I was caught off guard since I had already moved on to working on other implementations and features; I did not want to go back and deal with it anymore, yet it needed to be done.

It seemed to be a time-consuming task, I thought to myself. I started Googling for possible solutions, then I found what exactly I was looking for on [How to set currentTime in video.js in Safari for iOS](https://stackoverflow.com/questions/28823567/how-to-set-currenttime-in-video-js-in-safari-for-ios/47332408#47332408).

It turned out that in order to auto skip to a point of video time on **iOS Safari**, a video needs to be played first. By creating an event **canplaythrough** to check a video while being played, we can create a boolean variable to check if a video has already been started; if yes, we can force a current time to jump to given start time.

```javascript
var bool_is_initiation_done = false;
obj_player.on("canplaythrough", function () {
  if (!bool_is_initiation_done) {
    obj_player.currentTime(i_start_time);
    bool_is_initiation_done = true;
  }
});
```

Eventually, I had a function that works across web browsers for the time being. Voilà!

```javascript
// Utility
function common_util_value_is_null(exp) {
  return typeof exp === "undefined" || exp === null || exp == undefined;
}

// Set time marker
function layout_util_file_display_movie_set_time_marker(obj_player, i_time) {
  if (common_util_value_is_null(i_time)) return false;
  str_video_plugin_id = "video_plugin_name_example";
  obj_player[str_video_plugin_id].addMark(i_time, "general");
}

// Set timerange (starttime, endtime)
function layout_util_file_display_movie_set_timerange(
  obj_player,
  arr_timerange
) {
  i_start_time = arr_timerange[0];
  i_end_time = arr_timerange[arr_timerange.length - 1];

  // Set starttime and endtime
  // Remark: endtime marker MUST NOT exceed video's length; otherwise, it will create a video display overflow
  obj_player.on("loadedmetadata", function () {
    if (
      !common_util_value_is_null(i_start_time) &&
      i_start_time >= 0 &&
      i_start_time <= obj_player.duration()
    ) {
      layout_util_file_display_movie_set_time_marker(obj_player, i_start_time);
      obj_player.currentTime(i_start_time);
    }
    if (
      !common_util_value_is_null(i_end_time) &&
      i_end_time > i_start_time &&
      i_end_time <= obj_player.duration()
    ) {
      layout_util_file_display_movie_set_time_marker(obj_player, i_end_time);
    }
  });

  // iPhone and iPad need to play first, then set the starttime
  var bool_is_initiation_done = false;
  obj_player.on("canplaythrough", function () {
    if (!bool_is_initiation_done) {
      obj_player.currentTime(i_start_time);
      bool_is_initiation_done = true;
    }
  });

  return false;
}
```

## Summary

Making something that works across multiple web browsers is challenging because of their different browser engines (e.g., WebKit, Gecko, Chromium, etc.). Yet, it is achievable. All we need is a set of right tools and proper information that can be found all over the Internet nowadays.

Lastly, over-engineering is a productivity killer. Not all implementations are worth spending much time with, knowing when to drop them out is always a good habit.

I hope this article is helpful for everyone, cheers.
