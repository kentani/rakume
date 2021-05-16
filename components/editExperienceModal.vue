<template>
  <v-card>
    <v-card-title>
      <span class="headline">Experience</span>
    </v-card-title>
    <v-card-text class="pb-0">
      <v-text-field
        v-model="experiencePeriod"
        filled
        label="期間"
        placeholder="期間を入力"
        :color="color"
      />
      <v-text-field
        v-model="experienceProject"
        filled
        label="プロジェクト"
        placeholder="プロジェクトを入力"
        :color="color"
      />
      <v-textarea
        v-model="experienceContent"
        filled
        label="概要"
        placeholder="概要を入力"
        :color="color"
      />
    </v-card-text>
    <v-card-actions>
      <v-spacer />
      <v-btn text @click="closeExperienceModal">close</v-btn>
      <v-btn text @click="saveExperienceModal">save</v-btn>
    </v-card-actions>
  </v-card>
</template>

<script>
export default {
  props: {
    experience: {
      type: Object,
      require: true
    },
    color: {
      type: String,
      default: 'dark'
    },
  },
  computed: {
    experience2() {
      return this.experience 
    },
    experiencePeriod: {
      get: function () {
        return this.experience2.period
      },
      set: function (newValue) {
        this.experience2.period = newValue
      }
    },
    experienceProject: {
      get: function () {
        return this.experience2.project
      },
      set: function (newValue) {
        this.experience2.project = newValue
      }
    },
    experienceContent: {
      get: function () {
        return this.experience2.content
      },
      set: function (newValue) {
        this.experience2.content = newValue
      }
    },
  },
  methods: {
    closeExperienceModal() {
      this.experiencePeriod = this.experience.period
      this.experienceProject = this.experience.project
      this.experienceContent = this.experience.content
      this.$emit("handleEditExperienceClosed")
    },
    saveExperienceModal() {
      this.$emit("handleEditExperienceSaved", { period: this.experience.period, project: this.experience.project, content: this.experience.content })
    }
  }
}
</script>