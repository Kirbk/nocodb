<template>
  <v-responsive @contextmenu="showMenuFn">
    <!-- <v-layout flex class="project-logs" @contextmenu="showMenuFn">
      <v-flex xs12
      > -->
    <v-data-table
      dense
      :headers="headers"
      :items="logs"
      class="elevation-1 project-logs small-footer"
      flex
      footer-props.items-per-page-options="100"
    >
      <template v-slot:item="props">
        <tr class="" style="height:19px !important;">
          <td class="py-0">
            <v-icon
              v-if="props.item.status === 0"
              color="green lighten-1"
              size="10"
            >mdi-circle
            </v-icon>
            <v-icon
              size="10" v-else-if="props.item.status === 1" color="orange lighten-1"
            >mdi-circle
            </v-icon>
            <v-icon
              size="10" v-else color="red lighten-1">mdi-circle
            </v-icon>
          </td>
          <!--        <td>{{ 100-props.index }}</td>-->
          <td  class="caption grey--text py-0">{{ props.item.time }}</td>
          <td  class="caption py-0">
            {{ props.item.action }}
          </td>
        </tr>
        <!--        <td>{{ props.item.response }}</td>-->
        <!--        <td>{{ props.item.duration }}</td>-->
      </template>
    </v-data-table>
    <!-- </v-flex
      > -->
    <v-menu
      v-if="showMenu"
      v-model="showMenu"
      absolute
      :position-x="x"
      :position-y="y"
      offset-y
    >
      <v-list>
        <v-list-item @click="clearLogs">
          <v-list-item-title>Clear Logs</v-list-item-title>
        </v-list-item>
      </v-list>
    </v-menu>
    <!-- </v-layout> -->
  </v-responsive>
</template>

<script>
  // const {emittery} = require("electron").remote.require("./libs");
  import {mapGetters, mapActions, mapState} from "vuex";

  export default {
    components: {},
    data() {
      return {
        showMenu: false,
        x: 0,
        y: 0,
        headers: [
          {
            text: "",
            sortable: false,
            width: "1%",
            class: ""
          },
          // {
          //   text: "#",
          //   sortable: false,
          //   width: "1%",
          //   class: "grey darken-2"
          // },
          {
            text: "Time",
            sortable: false,
            width: "1%",
            class: "caption"
          },
          {
            text: "Logs",
            sortable: false,
            width: "100%",
            class: "caption"
          }
          // {
          //   text: "Response",
          //   sortable: false,
          //   width: "1%",
          //   class: ""
          // },
          // {
          //   text: "Duration",
          //   sortable: false,
          //   width: "10%",
          //   class: ""
          // }
        ],
        logs: [],
        maxLogs: 100
      };
    },
    methods: {
      showMenuFn(e) {
        // console.log("showMenuFn", e);
        e.preventDefault();
        this.showMenu = false;
        this.x = e.clientX;
        this.y = e.clientY;
        this.$nextTick(() => {
          this.showMenu = true;
        });
      },
      clearLogs() {
        this.logs = [];
      }
    },
    computed: {},

    beforeCreated() {
    },
    created() {
    },
    mounted() {
      // emittery.on("UI", data => {
      //   //const logs = JSON.parse(JSON.stringify(this.logs));
      //   if (this.logs.length > this.maxLogs) {
      //     this.logs.pop();
      //   }
      //   this.logs.unshift({
      //     status: data.status,
      //     action: data.data,
      //     time: new Date().toLocaleTimeString()
      //     // response: "",
      //     // duration: "123 ms"
      //   });
      //   //this.logs = logs;
      // });
    },
    beforeDestroy() {
    },
    destroy() {
    },
    validate({params}) {
      return true;
    },
    head() {
      return {};
    },
    props: {},
    watch: {},
    directives: {}
  };
</script>

<style scoped>
</style>
<!--
/**
 * @copyright Copyright (c) 2021, Xgene Cloud Ltd
 *
 * @author Naveen MR <oof1lab@gmail.com>
 * @author Pranav C Balan <pranavxc@gmail.com>
 *
 * @license GNU AGPL version 3 or any later version
 *
 * This program is free software: you can redistribute it and/or modify
 * it under the terms of the GNU Affero General Public License as
 * published by the Free Software Foundation, either version 3 of the
 * License, or (at your option) any later version.
 *
 * This program is distributed in the hope that it will be useful,
 * but WITHOUT ANY WARRANTY; without even the implied warranty of
 * MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
 * GNU Affero General Public License for more details.
 *
 * You should have received a copy of the GNU Affero General Public License
 * along with this program. If not, see <http://www.gnu.org/licenses/>.
 *
 */
-->
