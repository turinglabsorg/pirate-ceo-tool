<template>
  <div class="hello">
    <div id="preview">
      <img
        src="https://opengameart.org/sites/default/files/Pirate%20Captain%20%28Idle%29%20PREVIEW.png"
        width="350"
        height="350"
      />
    </div>
    <br /><br />
    <input type="text" v-if="!isTaking" v-model="text" style="font-size:16px; width:364px; padding:10px 5px"><br>
    <button
      v-on:click="takeGif"
      v-if="!isTaking"
      style="font-size: 22px; padding: 10px 25px"
    >
      TAKE GIF</button
    >
    <button
      v-on:click="downloadFile"
      v-if="image"
      style="font-size: 22px; padding: 10px 25px"
    >
      DOWNLOAD GIF</button
    ><br />
    <div v-if="isTaking && remaining > 0" style="font-size: 30px">
      <span style="font-size: 18px">{{ isTaking }}</span
      ><br />
      -{{ remaining }}s
    </div>
    <div v-if="isTaking && remaining === 0" style="font-size: 30px">
      Creating gif, please wait..
    </div>
  </div>
</template>

<script>
export default {
  name: "TakeGif",
  data() {
    return {
      isTaking: "",
      image: "",
      remaining: 0.4,
      text: "I'm the CEO of PirateChain"
    };
  },
  methods: {
    takeGif() {
      const app = this;
      document.getElementById("preview").innerHTML =
        '<img src="https://opengameart.org/sites/default/files/Pirate%20Captain%20%28Idle%29%20PREVIEW.png" width="350" height="350">';
      app.remaining = 0.5;
      app.isTaking = "Please wait until camera finish recording..";
      app.image = "";
      var interval = setInterval(function () {
        let less = parseFloat(app.remaining) - 0.01;
        if (less > 0) {
          app.remaining = less.toFixed(2);
        } else {
          app.remaining = 0;
        }
      }, 100);
      window.gifshot.createGIF(
        {
          keepCameraOn: true,
          interval: 0.3,
          fontSize: "22px",
          gifWidth: 400,
          gifHeight: 400,
          textYCoordinate: 360,
          text: app.text,
        },
        function (obj) {
          document.getElementById("preview").innerHTML = "";
          if (!obj.error) {
            var image = obj.image,
              animatedImage = document.createElement("img");
            animatedImage.src = image;
            app.image = image;
            document.getElementById("preview").appendChild(animatedImage);
            app.isTaking = "";
            clearInterval(interval);
          } else {
            app.isTaking = "Recording errored, please allow camera!";
          }
        }
      );
    },
    downloadFile() {
      const app = this;
      const linkSource = app.image;
      const downloadLink = document.createElement("a");
      document.body.appendChild(downloadLink);
      const fileName = "Pirate_CEO.gif";
      downloadLink.href = linkSource;
      downloadLink.target = "_self";
      downloadLink.download = fileName;
      downloadLink.click();
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
