<template>
  <div class="about">
    <div id="jitsi-container"></div>
  </div>
</template>
 <script src='https://meet.jit.si/external_api.js'></script>
<script>
//import JitsiMeetExternalAPI from "../store/metting";
export default {
  data() {
    return {
      room: "test romm",
      username: "rabie"
    };
  },
  methods: {
    startConference() {
      var _this = this;
      try {
        const domain = "meet.jit.si";
        const options = {
          audioInput: "<deviceLabel>",
          audioOutput: "<deviceLabel>",
          videoInput: "<deviceLabel>",
          roomName: this.room,
          height: 500,
          parentNode: document.getElementById("jitsi-container"),
          interfaceConfigOverwrite: {
            filmStripOnly: false,
            SHOW_JITSI_WATERMARK: false
          },
          configOverwrite: {
            disableSimulcast: false
          }
        };

        this.api = new JitsiMeetExternalAPI(domain, options);
        this.api.addEventListener("videoConferenceJoined", () => {
          console.log("Local User Joined");

          _this.api.executeCommand("displayName", _this.username);
        });
      } catch (error) {
        console.error("Failed to load Jitsi API", error);
      }
    },
    openRoom() {
      // verify the JitsiMeetExternalAPI constructor is added to the global..
      if (window.JitsiMeetExternalAPI) {
        var person = prompt("Please enter your name:", "Rabie");
        if (person != null || person != "") this.username = person;
        var room = prompt("Please enter your room:", "Test Room");
        if (room != null || room != "") this.room = room;
        this.startConference();
      } else alert("Jitsi Meet API script not loaded");
    }
  },

  mounted() {
    this.openRoom();
  }
};
</script>
