<template>
  <div>
    <n-card hoverable>
      <template #header> Server Info </template>
      <n-table style="max-width: 500px; border: none">
        <tbody>
          <template v-for="data in tableData">
            <tr>
              <td>{{ data.key }}</td>
              <td>
                <n-button text @click="copySomething(data.value)">{{
                  data.value
                }}</n-button>
              </td>
            </tr>
          </template>
        </tbody>
      </n-table>
    </n-card>

    <n-card v-if="sponsorMessage.length > 0" hoverable style="margin-top: 10px">
      <template #header> Node Sponsor </template>
      <div class="sponsor">
        <Markdown :source="sponsorMessage" />
      </div>
    </n-card>
  </div>
</template>

<script>
import { defineComponent } from "vue";
import useClipboard from "vue-clipboard3";
import { useMessage } from "naive-ui";
import Markdown from "vue3-markdown-it";

const { toClipboard } = useClipboard();

export default defineComponent({
  components: {
    Markdown,
  },
  props: {
    wsMessage: Array,
    componentConfig: Object,
  },
  data() {
    return {
      tableData: [],
      sponsorMessage: "",
      locationMap: {
  "中国": "China",
  "美国": "United States",
  "加拿大": "Canada",
  "英国": "United Kingdom",
  "法国": "France",
  "德国": "Germany",
  "澳大利亚": "Australia",
  "新西兰": "New Zealand",
  "日本": "Japan",
  "韩国": "South Korea",
  "印度": "India",
  "巴西": "Brazil",
  "阿根廷": "Argentina",
  "墨西哥": "Mexico",
  "俄罗斯": "Russia",
  "南非": "South Africa",
  "沙特阿拉伯": "Saudi Arabia",
  "阿联酋": "United Arab Emirates",
  "土耳其": "Turkey",
  "埃及": "Egypt",
  "挪威": "Norway",
  "瑞典": "Sweden",
  "丹麦": "Denmark",
  "芬兰": "Finland",
  "荷兰": "Netherlands",
  "比利时": "Belgium",
  "奥地利": "Austria",
  "瑞士": "Switzerland",
  "爱尔兰": "Ireland",
  "卢森堡": "Luxembourg",
  "意大利": "Italy",
  "希腊": "Greece",
  "西班牙": "Spain",
  "葡萄牙": "Portugal",
  "冰岛": "Iceland",
  "马耳他": "Malta",
  "塞浦路斯": "Cyprus",
  "立陶宛": "Lithuania",
  "拉脱维亚": "Latvia",
  "爱沙尼亚": "Estonia",
  "波兰": "Poland",
  "捷克": "Czech Republic",
  "斯洛伐克": "Slovakia",
  "匈牙利": "Hungary",
  "斯洛文尼亚": "Slovenia",
  "克罗地亚": "Croatia",
  "波黑": "Bosnia and Herzegovina",
  "塞尔维亚": "Serbia",
  "黑山": "Montenegro",
  "北马其顿": "North Macedonia",
  "阿尔巴尼亚": "Albania",
  "保加利亚": "Bulgaria",
  "摩尔多瓦": "Moldova",
  "罗马尼亚": "Romania",
  "乌克兰": "Ukraine",
  "白俄罗斯": "Belarus",
  "格鲁吉亚": "Georgia",
  "亚美尼亚": "Armenia",
  "阿塞拜疆": "Azerbaijan",
  "哈萨克斯坦": "Kazakhstan",
  "乌兹别克斯坦": "Uzbekistan",
  "吉尔吉斯斯坦": "Kyrgyzstan",
  "塔吉克斯坦": "Tajikistan",
  "土库曼斯坦": "Turkmenistan",
  "巴基斯坦": "Pakistan",
  "孟加拉国": "Bangladesh",
  "斯里兰卡": "Sri Lanka",
  "尼泊尔": "Nepal",
  "马尔代夫": "Maldives",
  "不丹": "Bhutan",
  "阿富汗": "Afghanistan",
  "伊朗": "Iran",
  "伊拉克": "Iraq",
  "约旦": "Jordan",
  "以色列": "Israel",
  "黎巴嫩": "Lebanon",
  "叙利亚": "Syria",
  "科威特": "Kuwait",
  "卡塔尔": "Qatar",
  "巴林": "Bahrain",
  "阿曼": "Oman",
  "也门": "Yemen",
  "巴勒斯坦": "Palestine",
  "印尼": "Indonesia",
  "马来西亚": "Malaysia",
  "新加坡": "Singapore",
  "泰国": "Thailand",
  "菲律宾": "Philippines",
  "越南": "Vietnam",
  "柬埔寨": "Cambodia",
  "老挝": "Laos",
  "缅甸": "Myanmar",
  "文莱": "Brunei",
  "东帝汶": "East Timor",
  "蒙古": "Mongolia",
  "朝鲜": "North Korea",
  "韩国": "South Korea",
  "台湾": "Taiwan",
  "香港": "Hong Kong",
  "澳门": "Macau",
  "安道尔": "Andorra",
  "摩纳哥": "Monaco",
  "圣马力诺": "San Marino",
  "梵蒂冈": "Vatican City",
  "列支敦士登": "Liechtenstein",
  "马尔他": "Malta",
  "摩尔多瓦": "Moldova",
  "马其顿": "Macedonia",
  "亚美尼亚": "Armenia",
  "白俄罗斯": "Belarus",
  "阿塞拜疆": "Azerbaijan",
  "乔治亚": "Georgia",
  "哈萨克斯坦": "Kazakhstan",
  "摩洛哥": "Morocco",
  "阿尔及利亚": "Algeria",
  "突尼斯": "Tunisia",
  "利比亚": "Libya",
  "苏丹": "Sudan",
  "西撒哈拉": "Western Sahara",
  "毛里塔尼亚": "Mauritania",
  "塞内加尔": "Senegal",
  "冈比亚": "Gambia",
  "几内亚比绍": "Guinea-Bissau",
  "几内亚": "Guinea",
  "塞拉利昂": "Sierra Leone",
  "利比里亚": "Liberia",
  "科特迪瓦": "Ivory Coast",
  "加纳": "Ghana",
  "布基纳法索": "Burkina Faso",
  "马里": "Mali",
  "尼日尔": "Niger",
  "尼日利亚": "Nigeria",
  "乍得": "Chad",
  "喀麦隆": "Cameroon",
  "中非共和国": "Central African Republic",
  "赤道几内亚": "Equatorial Guinea",
  "加蓬": "Gabon",
  "刚果共和国": "Republic of the Congo",
  "刚果民主共和国": "Democratic Republic of the Congo",
  "乌干达": "Uganda",
  "肯尼亚": "Kenya",
  "卢旺达": "Rwanda",
  "布隆迪": "Burundi",
  "坦桑尼亚": "Tanzania",
  "津巴布韦": "Zimbabwe",
  "赞比亚": "Zambia",
  "马拉维": "Malawi",
  "莫桑比克": "Mozambique",
  "马达加斯加": "Madagascar",
  "塞舌尔": "Seychelles",
  "科摩罗": "Comoros",
  "毛里求斯": "Mauritius",
  "斯威士兰": "Swaziland",
  "莱索托": "Lesotho",
  "纳米比亚": "Namibia",
  "博茨瓦纳": "Botswana",
  "南非": "South Africa",
  "安哥拉": "Angola",
  "圣多美和普林西比": "Sao Tome and Principe",
  "加纳利群岛": "Canary Islands",
  "特立尼达和多巴哥": "Trinidad and Tobago",
  "圣基茨和尼维斯": "Saint Kitts and Nevis",
  "巴巴多斯": "Barbados",
  "圣卢西亚": "Saint Lucia",
  "圣文森特和格林纳丁斯": "Saint Vincent and the Grenadines",
  "格林纳达": "Grenada",
  "巴哈马": "Bahamas",
  "安提瓜和巴布达": "Antigua and Barbuda",
  "多米尼克": "Dominica",
  "哈瓦那": "Havana",
  "玻利维亚": "Bolivia",
  "哥伦比亚": "Colombia",
  "委内瑞拉": "Venezuela",
  "圭亚那": "Guyana",
  "苏里南": "Suriname",
  "厄瓜多尔": "Ecuador",
  "秘鲁": "Peru",
  "巴拉圭": "Paraguay",
  "智利": "Chile",
  "阿根廷": "Argentina",
  "乌拉圭": "Uruguay",
  "南乔治亚岛和南桑德韦奇岛": "South Georgia and the South Sandwich Islands",
  "斐济": "Fiji",
  "帕劳": "Palau",
  "密克罗尼西亚": "Micronesia",
  "马绍尔群岛": "Marshall Islands",
  "基里巴斯": "Kiribati",
  "瑙鲁": "Nauru",
  "图瓦卢": "Tuvalu",
  "萨摩亚": "Samoa",
  "汤加": "Tonga",
  "瓦努阿图": "Vanuatu",
  "所罗门群岛": "Solomon Islands",
  "巴布亚新几内亚": "Papua New Guinea",
  "密克罗尼西亚联邦": "Federated States of Micronesia",
  "库克群岛": "Cook Islands",
  "波利尼西亚": "Polynesia",
  "皮特凯恩群岛": "Pitcairn Islands",
  "北马里亚纳群岛": "Northern Mariana Islands",
  "关岛": "Guam",
  "美属萨摩亚": "American Samoa",
  "纽埃": "Niue",
  "托克劳": "Tokelau",
  "瓦利斯和富图纳": "Wallis and Futuna",
  "法属波利尼西亚": "French Polynesia",
  "新喀里多尼亚": "New Caledonia"
}
    };
  },
  methods: {
    copySomething(String) {
      toClipboard(String, "copy");
      window.$message.success("Copied");
    },
  },
  setup() {
    window.$message = useMessage();
  },
  mounted() {
    let DataWatcher = this.$watch(
      () => this.wsMessage,
      () => {
        this.wsMessage.forEach((e, i) => {
          if (e[0] != 1000) return;
          let data = JSON.parse(e[1]);
          this.wsMessage.splice(i, 1);
          this.componentConfig.public_ipv4 = data.public_ipv4;
          this.componentConfig.public_ipv6 = data.public_ipv6;
          this.componentConfig.testFiles = data.testfiles;
          this.componentConfig.display_traffic = data.display_traffic;
          this.componentConfig.display_speedtest = data.display_speedtest;
          this.componentConfig.utilities_ping = data.utilities_ping;
          this.componentConfig.utilities_traceroute = data.utilities_traceroute;
          this.componentConfig.utilities_iperf3 = data.utilities_iperf3;
          this.componentConfig.utilities_speedtestdotnet =
            data.utilities_speedtestdotnet;
          this.componentConfig.utilities_fakeshell = data.utilities_fakeshell;

          this.tableData = [];
          this.tableData.push({
            key: "Server Location",
            value: this.locationMap[data.location] || data.location,
          });

          if (data.public_ipv4) {
            this.tableData.push({
              key: "IPv4 Address",
              value: data.public_ipv4,
            });
          }

          if (data.public_ipv6) {
            this.tableData.push({
              key: "IPv6 Address",
              value: data.public_ipv6,
            });
          }

          this.tableData.push({
            key: "Your Current IP Address",
            value: data.client_ip,
          });

          if (data.sponsor_message.length > 0) {
            this.sponsorMessage = data.sponsor_message;
          }
          DataWatcher();
        });
      },
      { immediate: true, deep: true }
    );
  },
});
</script>

<style>
.sponsor a {
  color: #70c0e8;
  text-decoration: none;
}
</style>
