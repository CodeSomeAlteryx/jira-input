<template>

  <div>

    <v-alert outline type="error" :value="showAlert" class="mb-3 mt-3">
      {{ alertText }}
    </v-alert>

    <v-list dense two-line v-if="!showAlert"class="pt-0">
      <v-subheader v-text="projects.length + ' projects available, ' + selected.length + ' selected'"></v-subheader>
      <template v-for="project in projects">
        <v-list-tile avatar :key="project.id" @click="">
          <v-list-tile-avatar>
            <img :src="project.avatarUrls['32x32']">
          </v-list-tile-avatar>
          <v-list-tile-content>
            <v-list-tile-title>
              {{ project.name }}
            </v-list-tile-title>
            <v-list-tile-sub-title>
              <span class="blue-grey--text text--lighten-1">{{ project.key }}</span>
              <span v-if="project.description.length > 0">
                <span class="ml-1 mr-1 grey--text">-</span>
                {{ project.description }}
              </span>
            </v-list-tile-sub-title>
          </v-list-tile-content>
          <v-list-tile-action>
            <v-checkbox v-model="selected" :value="project.id" @change="updateSelections(project.name, project.id)"></v-checkbox>
          </v-list-tile-action>
        </v-list-tile>
        <v-divider inset></v-divider>
      </template>
    </v-list>

  </div>

</template>

<script>
  export default {
  	name: 'JiraProjects',
    data() {
      return {
        selected: this.$store.state.ui.selections
      }
    },
    computed: {
      page() {
        return this.$store.state.ui.page
      },
      showAlert() {
        if (this.$store.state.ui.connected == 0 || this.$store.state.ui.connectError > 0) {
          return true
        } else if (this.$store.state.ui.auth !== this.$store.state.ui.lastAuth || this.$store.state.ui.url !== this.$store.state.ui.lastUrl) {
          return true
        } else {
          return false
        }
      },
      alertText () {
        if (this.$store.state.ui.connected == 0 || this.$store.state.ui.connectError > 0) {
          return 'Whoops!  Not connected to a Jira instance...'
        } else if (this.$store.state.ui.auth !== this.$store.state.ui.lastAuth || this.$store.state.ui.url !== this.$store.state.ui.lastUrl) {
          return 'Connection details have changed without being verified...'
        } else {
          return 'Good to go!'
        }
      },
      projects () {
        return this.$store.state.ui.projects
      }
    },
    methods: {
      updateSelections (a,b) {
        this.$store.commit('updateSelections',this.selected)
      }
    }
  }
</script>

