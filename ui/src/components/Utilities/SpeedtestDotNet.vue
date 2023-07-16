<template>
  <n-space vertical>
    <n-input-group>
      <n-input
        :disabled="working"
        v-model:value="serverId"
        :style="{ width: '90%' }"
        placeholder="speedtest.net Server ID (optional)"
        @keyup.enter="speedtest"
      />
      <n-button :loading="working" type="primary" ghost @click="speedtest()">
        Run
      </n-button>
    </n-input-group>
    <n-collapse-transition :show="isQueue">
      <n-spin>
        <n-alert :show-icon="false" :bordered="false">
          <br />
          <br />
        </n-alert>
        <template #description>
          Request in queue, position {{ QueueStat.pos }} out of {{ QueueStat.total }}
        </template>
      </n-spin>
    </n-collapse-transition>

    <n-collapse-transition :show="isSpeedtest && action == '' && !isCrash">
      <n-alert :show-icon="false" :bordered="false"> Test will begin shortly... </n-alert>
    </n-collapse-transition>
    <n-collapse-transition :show="speedtestData.result != ''">
      <n-alert :show-icon="false" :bordered="false">
        <a :href="speedtestData.result" target="_blank">
          <img
            :src="speedtestData.result + '.png'"
            style="max-width: 300px; height: 100%; display: flex; margin: auto"
          />
        </a>
      </n-alert>
    </n-collapse-transition>
    <n-collapse-transition :show="isSpeedtest && action != ''">
      <n-collapse-transition :show="working">
        <p>
          {{ action }} - Progress
          <span style="float: right">{{ progress.sub }}%</span>
        </p>
        <n-progress
          type="line"
          :percentage="progress.sub"
          :show-indicator="false"
          :processing="working"
        />
        <p>
          Total Progress <span style="float: right">{{ progress.full }}%</span>
        </p>
        <n-progress
          type="line"
          :percentage="progress.full"
          :show-indicator="false"
          :processing="working"
        />
      </n-collapse-transition>
      <n-collapse-transition
        :show="isSpeedtest && speedtestData.serverInfo.id != ''"
      >
        <n-divider v-if="working" />
        <n-table :bordered="true" :single-line="false">
          <tbody>
            <tr>
              <td>Server ID</td>
              <td>{{ speedtestData.serverInfo.id }}</td>
            </tr>
            <tr>
              <td>Server Location</td>
              <td>{{ speedtestData.serverInfo.pos }}</td>
            </tr>
            <tr>
              <td>Server Name</td>
              <td>{{ speedtestData.serverInfo.name }}</td>
            </tr>
          </tbody>
        </n-table>
      </n-collapse-transition>

      <n-collapse-transition :show="isSpeedtest && speedtestData.ping != '0'">
        <n-divider />
        <n-table :bordered="true" :single-line="false">
          <tbody>
            <tr>
              <td>Latency</td>
              <td v-if="speedtestData.ping == '0'">Waiting to Start</td>
              <td v-else>{{ speedtestData.ping }} ms</td>
            </tr>
            <tr>
              <td>Download Speed</td>
              <td v-if="speedtestData.download == ''">Waiting to Start</td>
              <td v-else>{{ speedtestData.download }}</td>
            </tr>
            <tr>
              <td>Upload Speed</td>
              <td v-if="speedtestData.upload == ''">Waiting to Start</td>
              <td v-else>{{ speedtestData.upload }}</td>
            </tr>
          </tbody>
        </n-table>
      </n-collapse-transition>
    </n-collapse-transition>
  </n-space>
</template>

<script>
import { defineComponent, defineAsyncComponent } from "vue";
export default defineComponent({
  data() {
    return {
      isCrash: false,
      isQueue: false,
      QueueStat: {
        pos: 0,
        total: 0,
      },
      isSpeedtest: false,
      serverId: "",
      working: false,
      action: "",
      isFinish: false,
      progress: {
        sub: 0,
        full: 0,
      },
      speedtestData: {
        ping: "0",
        download: "",
        upload: "",
        result: "",
        serverInfo: {
          id: "",
          name: "",
          pos: "",
        },
      },
      steps: {
        start: false,
        ping: false,
        download: false,
        upload: false,
        end: false,
      },
    };
  },
  props: {
    wsMessage: Array,
    ws: WebSocket,
    componentConfig: Object,
  },
  methods: {
    formatBytes(bytes, decimals = 2, bandwidth = false) {
      if (bytes === 0) return "0 Bytes";

      const k = 1024;
      const dm = decimals < 0 ? 0 : decimals;
      const sizes = ["Bytes", "KB", "MB", "GB", "TB", "PB", "EB", "ZB", "YB"];
      const bandwidthSizes = [
        "Bps",
        "Kbps",
        "Mbps",
        "Gbps",
        "Tbps",
        "Pbs",
        "Ebps",
        "Zbps",
        "Ybps",
      ];

      const i = Math.floor(Math.log(bytes) / Math.log(k));

      if (bandwidth) {
        bytes = bytes * 10;
        return (
          parseFloat((bytes / Math.pow(k, i)).toFixed(dm)) +
          " " +
          bandwidthSizes[i]
        );
      }
      return parseFloat((bytes / Math.pow(k, i)).toFixed(dm)) + " " + sizes[i];
    },
    speedtest() {
      const commandIndex = "5";
      if (this.working) return false;
      this.working = true;
      this.isSpeedtest = false;
      this.isCrash = false;
      this.isQueue = true;
      this.speedtestData = {
        ping: "0",
        download: "",
        upload: "",
        result: "",
        serverInfo: {
          id: "",
          pos: "",
          name: "",
        },
      };
      this.action = "";
      if (this.serverId.length == 0) {
        this.ws.send(commandIndex);
      } else {
        this.ws.send(commandIndex + "|" + this.serverId);
      }

      let ticket = "";
      let worker = this.$watch(
        () => this.wsMessage,
        (e) => {
          this.wsMessage.forEach((e, i) => {
            if (e[0] != commandIndex) return true;

            if (ticket.length == 0 && e[1] == "1" && e.length == 3) {
              ticket = e[2];
              this.wsMessage.splice(i, 1);
              return true;
            }

            if (ticket != e[1]) {
              this.wsMessage.splice(i, 1);
              return true;
            }

            if (e[2] == "2") {
              if (e[3] == "0") {
                this.isQueue = false;
                this.isSpeedtest = true;
                this.wsMessage.splice(i, 1);
                return true;
              }
              this.isQueue = true;
              this.QueueStat.pos = e[4];
              this.QueueStat.total = e[5];
              this.wsMessage.splice(i, 1);
              return true;
            }

            switch (e[2]) {
              // end
              case "0":
                this.working = false;
                worker();
                this.wsMessage.splice(i, 1);
                if (!this.isFinish) {
                  this.action = "";
                  this.isSpeedtest = false;
                  this.isCrash = true;
                }
                return false;
              case "3":
                this.progress.sub = parseInt(e[3]);
                break;
              case "4":
                this.progress.full = parseInt(e[3]);
                break;
              case "100":
                this.action = "Retrieving server information";
                break;
              case "101":
                this.action = "Testing server latency";
                break;
              case "102":
                this.action = "Testing download speed";
                break;
              case "103":
                this.action = "Testing upload speed";
                break;
              case "104":
                this.isFinish = true;
                break;
              case "200":
                let serverInfo = JSON.parse(e[3]);
                this.speedtestData.serverInfo.id = serverInfo.id;
                this.speedtestData.serverInfo.pos =
                  serverInfo.country + " - " + serverInfo.location;
                this.speedtestData.serverInfo.name = serverInfo.name;
                break;
              case "201":
                this.speedtestData.ping = e[3];
                break;
              case "202":
                this.speedtestData.download = this.formatBytes(e[3], 2, true);
                break;
              case "203":
                this.speedtestData.upload = this.formatBytes(e[3], 2, true);
                break;
              case "204":
                let result = JSON.parse(e[3]);
                this.speedtestData.result = result.url;
                break;
            }

            this.wsMessage.splice(i, 1);
            return true;
          });
        },
        { immediate: true, deep: true }
      );
    },
  },
});
</script>