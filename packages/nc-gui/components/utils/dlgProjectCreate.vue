<template>
  <v-dialog v-model="value" width="600px">
    <v-card>
      <v-container fluid>
        <v-form v-model="valid" ref="form">
          <div style="width:500px" class="mx-auto mt-10">
            <v-text-field
              @keyup.enter="createProject"
              ref="input"
              outlined
              :full-width="false"
              class="caption"
              dense
              label="Project name"
              v-model="name"
              :rules="[v => !!v || 'Project name required']"
            ></v-text-field>


            <div v-for="type in projectTypes">
              <v-radio-group hide-details dense v-model="projectType">
                <v-radio :value="type.value">
                  <template v-slot:label>
                    <v-icon small :color="type.iconColor">{{ type.icon }}</v-icon>
                    <span class="caption">{{ type.text }}</span>
                  </template>
                </v-radio>
              </v-radio-group>
            </div>


            <!--            <v-select
                          outlined
                          label="API type"
                          v-model="projectType" persistent-hint dense
                          class="caption mt-3"
                          :items="projectTypes">
                          <template v-slot:prepend-inner>
                            <img v-if="typeIcon.type === 'img'" :src="typeIcon.icon" style="width: 25px">
                            <v-icon v-else :color="typeIcon.iconColor" size="25">{{ typeIcon.icon }}</v-icon>
                          </template>
                          <template v-slot:item="{item}">
                                  <span class="caption d-flex align-center">
                                  <img v-if="item.type === 'img'" :src="item.icon" style="width: 30px">
                                    <v-icon v-else :color="item.iconColor">{{ item.icon }}</v-icon> &nbsp; {{ item.text }}</span>
                          </template>

                        </v-select>-->


            <div class="text-center">
              <v-btn
                :loading="loading"
                :disabled="!valid"
                @click="createProject"
                color="primary">
                Create Project
              </v-btn>
            </div>

          </div>
        </v-form>
      </v-container>
    </v-card>
  </v-dialog>
</template>

<script>
export default {
  name: "dlgProjectCreate",
  props: {
    value: Boolean
  }, data: () => ({
    valid: null,
    name: '',
    loading: false,
    projectType: 'rest',
    projectTypes: [
      {text: 'Automatic REST APIs on database', value: 'rest', icon: 'mdi-json', iconColor: 'green'},
      {text: 'Automatic GRAPHQL APIs on database', value: 'graphql', icon: 'mdi-graphql', iconColor: 'pink'},
      /*      {
              text: 'Automatic gRPC APIs on database',
              value: 'grpc',
              icon: require('@/assets/img/grpc-icon-color.png'),
              type: 'img'
            },*/
    ],
  }),
  mounted() {
    setTimeout(() => {
      this.$refs.input.$el.querySelector('input').focus()
    }, 100);
  },
  methods: {
    async createProject() {
      if (this.$refs.form.validate()) {
        this.loading = true;
        try {
          const result = await this.$store.dispatch('sqlMgr/ActSqlOp', [null, "projectCreateByWebWithXCDB", {
            title: this.name,
            projectType: this.projectType,
          }]);

          await this.$store.dispatch('project/ActLoadProjectInfo');

          this.projectReloading = false;

          if (this.$store.state.project.projectInfo.firstUser || this.$store.state.project.projectInfo.authType === 'masterKey') {
            return this.$router.push({
              path: `/user/authentication/signup`
            });
          }

          this.$router.push({
            path: `/nc/${result.id}`,
            query: {
              new: 1
            }
          });
          this.$emit('change', false)
        } catch (e) {
          this.$toast.error(e.message).goAway(3000)
        }
        this.loading = false;

      }
    }
  },
  computed: {
    typeIcon() {
      if (this.projectType) {
        return this.projectTypes.find(({value}) => value === this.projectType)
      } else {
        return {'icon': 'mdi-server', iconColor: 'primary'}
      }
    },
  }
}
</script>

<style scoped>

</style>
