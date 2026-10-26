
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

我们为 `ionic-audio` 插件添加了一个相应的组件。接下来，我们将能够通过 `FileTransfer` 插件下载歌曲：

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

请确保在 `@Component` 装饰器之前声明 `cordova` 变量。此外，我们需要创建一个 `Transfer()` 方法的实例，然后将 URL 和路径传递给它，它将在此路径下保存文件。目前我们设置了 `rootDirectory` 作为文件保存的位置。

接下来，我们将使用 Streaming 插件在应用程序内播放视频。Streaming 插件使用原生媒体播放器，它非常流畅，而不是在 `HTMLAudioElement` 内播放：

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

我们可以进一步扩展音乐播放器，增加更多功能，例如将歌曲添加为收藏、创建播放列表，甚至添加音乐控制插件，该插件将显示带有播放/暂停、上一首和下一首操作的媒体通知。随着 Cordova 插件的快速发展，可用的可能性是无限的。

到目前为止，我们主要关注的是 Ionic Native 封装器提供的功能，而不是官方插件。这样做是为了利用开发以及与现有 Ionic 应用程序集成的便利性。我建议大家都查阅官方插件文档，以查看任何新的改进和变化。此外，有时官方仓库的位置可能会发生变化，所以这样我们总能保持更新。Ionic 框架发布后，混合开发一直在稳步发展，Cordova 插件的开发也是如此，因此最好始终查看官方插件文档以了解新功能。

## Cordova 与 phonegap

关于 Cordova 和 phonegap，一直存在很多困惑和疑问。此外，许多人询问在 Ionic 中应该使用哪一个。虽然 Ionic 使用 Cordova，但我们肯定也可以使用 phonegap。许多人仍然不了解 Cordova，因为 phonegap 早于 Cordova 出现。让我们快速回顾一下它的历史：phonegap 是由 Nitobi 于 2009 年创建的。后来在 2011 年，Adobe 收购了 Nitobi 以及 phonegap 平台的权利。该平台的开源核心代码被捐赠给 Apache 组织以进行进一步开发。phonegap 也是免费且开源的。起初，Cordova 和 phonegap 之间几乎没有区别，但慢慢地 Adobe 开始在 phonegap 周围构建一套专有服务。PhoneGap Build 就是这些服务之一。Cordova 是驱动 phonegap 的引擎，类似于 webkit 是 Chrome 浏览器的引擎。关于使用哪个平台一直存在长期争论，开发者之间也存在利益冲突。许多人批评 phonegap 是 Cordova 的修补版本，只是利用 phonegap 的远程构建等功能。最终，这与 Ionic 无关，并且独立于这场争论。作为开发者，你可以自由使用其中任何一个。

如果你仍然想使用 phonegap 并利用其内置服务，那么在使用 phonegap 构建项目时可以轻松选择它。Ionic CLI 命令使用 Cordova，因此你需要安装和使用 Cordova。

## 总结

Cordova 插件将不断演进，我们需要确保跟上所引入的最新功能。随着 Ionic 进入市场，过去几年混合应用程序和插件的开发显著增加。这是因为更多人对其感兴趣，并且为了跟上最新技术，开发者开发了许多自定义插件。一个类似例子是蓝牙信标，以及开发者为了支持其不同格式而开发的多个插件。我们现在已经涵盖了插件开发，这是开发复杂混合应用程序的另一个重要方面。我们现在可以利用插件的力量来构建需要访问原生 API 的高级混合应用程序。在本章稍后部分，我们继续介绍了许多 Ionic CLI 命令，这些命令帮助我们快速开发和调试应用程序。Ionic CLI 日新月异，许多新功能和任务即将推出。我们都应该定期更新 Ionic CLI 并查看新功能，以便在下一个应用程序中加以利用。

在第4章，*Ionic 平台与服务*中，我们将介绍 Ionic 服务和工具。在过去两年中，Ionic 发展了很多，Ionic 团队构建了非常实用的工具，加速了开发时间。Ionic CLI 是最常用和最重要的工具之一。Ionic 作为一个平台提供 Ionic Push、Ionic Deploy、Ionic Analytics 和 Ionic Lab，以及 Creator 和 Playground 帮助快速构建原型。Ionic 团队正在非常努力地为我们带来所有这些很棒的东西，帮助我们为用户创新应用程序。
