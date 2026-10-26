```
this.source = this.navParams.get('url');
this.img = this.navParams.get('img');

this.singleTrack = {
  src: this.source,
  artist: this.source.slice(25,-4),
  title: this.source.slice(25,-4),
  art: this.img,
  preload: 'metadata' // tell the plugin to preload
  metadata such as duration for this track, set to 'none' to turn off
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