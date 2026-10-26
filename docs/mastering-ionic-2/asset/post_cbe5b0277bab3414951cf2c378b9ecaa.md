
```
this.source = this.navParams.get('url');
this.img = this.navParams.get('img');

this.singleTrack = {
  src: this.source,
  artist: this.source.slice(25,-4),
  title: this.source.slice(25,-4),
  art: this.img,
  preload: 'metadata' // tell the plugin to preload metadata such as duration for this track, set to 'none' to turn off
};

ngAfterContentInit() {
  // get all tracks managed by AudioProvider so we can
  // control playback via the API
  this.allTracks = this._audioProvider.tracks;
}

playSelectedTrack() {
  // use AudioProvider to control selected track
  this._audioProvider.play(this.selectedTrack);
}

pauseSelectedTrack() {
  // use AudioProvider to control selected track
  this._audioProvider.pause(this.selectedTrack);
}

onTrackFinished(track: any) {
  console.log('Track finished', track)
}
}

// src/pages/music-player/music-player.html

<ion-content padding>
  <ion-card>
    <img [src]="img"/>
    <ion-card-content>
      <ion-card-title>
        {{ source | trimurl }}
      </ion-card-title>

      <audio-track #track [track]="singleTrack">
        <ion-item [audioTrack]="track">
          <ion-thumbnail item-left>
            <img src="{{track.art}}">
            <audio-track-play light [audioTrack]="track">
              <ion-spinner></ion-spinner>
            </audio-track-play>
          </ion-thumbnail>

          <div item-content style="width:100%">
            <p><strong>{{track.title}}</strong> ◦ <em>{{track.artist}}</em></p>
            <audio-track-progress-bar duration progress [audioTrack]="track"></audio-track-progress-bar>
            <em style="font-size:.5em">Track ID: {{track.id}}</em>
          </div>
        </ion-item>
      </audio-track>
    </ion-card-content>
  </ion-card>
</ion-content>
```

![](img/c54e3488d7687ea0d26e8c2ce3cdfae3_210_0.png)

We have added a respective component for the `ionic-audio` plugin. Next we will be able to download the songs via the `FileTransfer` plugin:

```
// src/pages/music/music.ts
import { Transfer, TransferObject } from '@ionic-native/filetransfer';

declare var cordova: any;

@Component({
  selector: 'page-music',
  templateUrl: 'music.html'
})
export class MusicPage {

  constructor(private transfer: Transfer);

  download(song) {
    const fileTransfer: TransferObject = this.transfer.create();
    let url = song.url;

    fileTransfer.download(url,
      cordova.file.externalRootDirectory +
      song.name).then((entry) => {
      alert('download complete: ' + entry.toURL());
    }, (error) => {
      // handle error
    });
  }
}
```

Make sure you declare the `cordova` variable before the `@Component` decorator. Also, we need to create an instance of the `Transfer()` method and then pass the URL and path to it where it will save the file. We have set the `rootDirectory` currently where the files will save.

Next, we will be using the Streaming plugin for playing videos inside our application. The streaming plugin uses the Native media player, which is really smooth, rather than playing inside `HTMLAudioElement`:

```
// src/pages/video-player/video-player
import { Component } from '@angular/core';
import { NavController, NavParams } from 'ionic-angular';
import { StreamingMedia, StreamingVideoOptions } from '@ionic-native/streaming-media';

@Component({
  selector: 'page-video-player',
  templateUrl: 'video-player.html'
})

export class VideoPlayerPage {

  source: string;

  constructor(public navCtrl: NavController, public navParams: NavParams) {

    this.source = this.navParams.get('url');

    // Playing a video.
    let options: StreamingVideoOptions = {
      successCallback: () => { console.log('Video played') },
      errorCallback: (e) => { console.log('Error streaming') },
      orientation: 'landscape'
    };

    StreamingMedia.playVideo(this.source, options);
  }

  ionViewDidLoad() {
    console.log('ionViewDidLoad VideoPlayerPage');
  }

}
```

We can further extend the music player with more features such as adding songs as favorites, creating playlists, or even adding a music control plugin, which will display media notifications with play/pause, previous, and next actions. There are unlimited possibilities available as Cordova plugins are evolving quickly.

Until now, we have mostly focused on functions that the Ionic Native wrapper provides rather than an official plugin. The reason for this is to take advantage of the ease of development and integration with your existing Ionic applications. I recommend everyone to check out the official plugin documentations to see any new improvements and changes. Also, sometimes there might be a change in official repository location, so this way we will always be updated. Hybrid development is going at a good pace after the launch of the Ionic Framework, the same goes for the Cordova plugin development, so it's always good to verify with official plugin documentation for new features.

## Cordova versus phonegap

There has been a lot of confusion and questions about Cordova and phonegap. Also, many people ask questions about which to use with Ionic. Although Ionic uses Cordova, we can also definitely use phonegap. Many still don't know about Cordova as phonegap is before Cordova. Let's quickly look back at its history; phonegap was created in 2009 by Nitobi. Later in 2011, Adobe took over Nitobi along with the rights to the phonegap platform. The Open source core code of the platform was donated to the Apache organization for further development. Phonegap is also free and open source. Initially, there was minimal difference between Cordova and phonegap, but slowly Adobe started building a proprietary set of services around phonegap. Phonegap build is one of those services. Cordova is the engine that powers phonegap, similar to webkit, which is the engine for the Chrome browser. There have been long debates upon which platform to use, there have been conflicts of interest between developers. Many criticize phonegap as a patch up version of Cordova, only using phonegap for its features such as the remote build option. At the end this is nothing to do with Ionic and it's independent of this debate. As a developer, you are free to use any of these.

If you still want to use phonegap and take advantage of the services built, you can easily choose that while building the project with phonegap. Ionic CLI commands use Cordova, so you will need to install and use Cordova.

## Summary

There will be a continuous evolution of Cordova plugins and we need to take care that we are updated with the latest features that are introduced. With Ionic coming into the market, the last few years have seen increased development of hybrid applications and plugins. This is because more people are interested in using it and multiple custom plugins are developed for keeping in touch with the latest technology. One similar example is Bluetooth beacons and multiple plugins developed by developers for supporting its different formats. We have now covered plugin development, which is another important aspect of developing complex hybrid applications. We can now leverage the power of plugins for building advance hybrid applications that require access to Native APIs. Later in the chapter, we went on to cover many Ionic CLI commands that help us quickly develop and debug applications. Ionic CLI is advancing day by day and many new features and tasks are coming. What we all should do is regularly update Ionic CLI and check for new features so that in our next applications we can take advantage of that.

In the Chapter 4, *Ionic Platform and Services*, we will look at Ionic services and tools. Over the last two years, Ionic has evolved a lot and the Ionic team has built really useful tools that speed up development time. Ionic CLI is one of the most used and important tools. Ionic as a platform offers Ionic push, Ionic deploy, Ionic analytics, and Ionic lab, along with creator and playground help to build prototypes quickly. The Ionic team is working really hard to bring us all this awesome stuff that helps us to make innovative applications for users.
