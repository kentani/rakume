<template>
  <div>
    <v-row justify="center" class="mb-6">
      <v-card tile width="1000">
        <v-card-actions class="px-0">
          <v-btn
            v-show="!isPreview"
            icon
            x-large
            :ripple="false"
            @click="settingDialog = true">
            <v-icon :color="color">mdi-square-edit-outline</v-icon>
          </v-btn>
          <v-btn
            v-for="colorName in colorList" :key="colorName"
            fab
            x-small
            depressed
            :ripple="false"
            :color="colorName"
            @click="changeColor(colorName)">
            <v-icon v-if="color == colorName" color="white">mdi-check</v-icon>
          </v-btn>
          <v-spacer />
          <v-btn
            v-show="!isPreview"
            icon
            x-large
            :ripple="false"
            @click="captureImage">
            <v-icon :color="color">mdi-download</v-icon>
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-row>
    <v-row justify="center">
      <v-card tile id="capture" :color="color" elevation="8" min-width="1000">
        <v-card-text>
          <v-row>
            <v-col cols="5" style="background-color: #EEEEEE; min-height: 100%;">
              <v-hover>
                <template v-slot:default="{ hover }">
                  <v-card flat class="mb-6" style="background-color: #EEEEEE; border: 2px solid; border-radius: 50px;">
                    <v-card-text>
                      <h2 class="display-1 grey--text text--darken-2 text-center font-weight-bold mb-0">{{ name }}</h2>
                    </v-card-text>
                    <v-fade-transition>
                      <v-overlay
                        v-if="hover"
                        absolute>
                        <v-btn rounded>編集する</v-btn>
                      </v-overlay>
                    </v-fade-transition>
                  </v-card>
                </template>
              </v-hover>
              <h3 class="title grey--text text--darken-2 font-weight-bold mb-0">HOBBY</h3>
              <v-hover>
                <template v-slot:default="{ hover }">
                  <v-card tile flat style="background-color: #EEEEEE; white-space: pre-line;">
                    <v-card-text class="body-2 grey--text text--darken-1 pa-2">
                      <p class="mb-0">{{ hobby }}</p>
                    </v-card-text>
                    <v-fade-transition>
                      <v-overlay
                        v-if="hover"
                        absolute
                        opacity="0.2">
                        <v-btn rounded>編集する</v-btn>
                      </v-overlay>
                    </v-fade-transition>
                  </v-card>
                </template>
              </v-hover>
              <v-divider class="mt-3 mb-6" style="border: 1px solid black; border-color: black;" />
              <h3 class="title grey--text text--darken-2 font-weight-bold mb-0">SKILLS</h3>
              <v-hover>
                <template v-slot:default="{ hover }">
                  <v-card tile flat style="background-color: #EEEEEE;">
                    <v-card-actions class="body-2 grey--text text--darken-1" v-for="skill in skills" :key="skill.item">
                      <span>{{ skill.item }}</span>
                      <v-spacer />
                      <v-rating
                        :value="skill.rating"
                        background-color="grey"
                        :color="color"
                        readonly
                        dense />
                    </v-card-actions>
                    <v-fade-transition>
                      <v-overlay
                        v-if="hover"
                        absolute
                        opacity="0.2">
                        <v-btn rounded>編集する</v-btn>
                      </v-overlay>
                    </v-fade-transition>
                  </v-card>
                </template>
              </v-hover>
            </v-col>
            <v-col cols="7" style="min-height: 100%;">
              <h3 class="title font-weight-bold white--text mb-0">PROFILE</h3>
              <v-hover>
                <template v-slot:default="{ hover }">
                  <v-card tile flat :color="color" style="white-space: pre-line;">
                    <v-card-text class="body-2 white--text pa-2">
                      <p class="mb-0">{{ profile }}</p>
                    </v-card-text>
                    <v-fade-transition>
                      <v-overlay
                        v-if="hover"
                        absolute
                        opacity="0.2">
                        <v-btn rounded>編集する</v-btn>
                      </v-overlay>
                    </v-fade-transition>
                  </v-card>
                </template>
              </v-hover>
              <v-divider class="mt-3 mb-6" style="border: 1px solid white; border-color: white;" />
              <h3 class="title font-weight-bold white--text mb-0">STRENGTH</h3>
              <v-hover>
                <template v-slot:default="{ hover }">
                  <v-card tile flat :color="color" style="white-space: pre-line;">
                    <v-card-text class="body-2 white--text pa-2">
                      <v-chip v-for="(strength, i) in strength" :key="i" class="mx-1" color="grey lighten-3">
                        <span class="body-2 grey--text text--darken-2">{{ i + 1 }}. {{ strength }}</span>
                      </v-chip>
                    </v-card-text>
                    <v-fade-transition>
                      <v-overlay
                        v-if="hover"
                        absolute
                        opacity="0.2">
                        <v-btn rounded>編集する</v-btn>
                      </v-overlay>
                    </v-fade-transition>
                  </v-card>
                </template>
              </v-hover>
              <v-divider class="mt-3 mb-6" style="border: 1px solid white; border-color: white;" />
              <h3 class="title font-weight-bold white--text mb-0">EXPERIENCE</h3>
              <v-hover>
                <template v-slot:default="{ hover }">
                  <v-card tile flat :color="color" style="white-space: pre-line;">
                    <v-card-text class="body-2 white--text pa-2">
                      <v-timeline dense>
                        <v-timeline-item
                          v-for="(experience, i) in experience"
                          :key="i"
                          :color="color"
                          small
                          right>
                          <div>
                            <div class="subtitle-2 white--text">{{ experience.period }}</div>
                            <h2 class="subtitle-1 font-weight-bold white--text">{{ experience.project }}</h2>
                            <div class="subtitle-2 white--text" style="white-space: pre-line;">{{ experience.content }}</div>
                          </div>
                        </v-timeline-item>
                      </v-timeline>
                    </v-card-text>
                    <v-fade-transition>
                      <v-overlay
                        v-if="hover"
                        absolute
                        opacity="0.2">
                        <v-btn rounded>編集する</v-btn>
                      </v-overlay>
                    </v-fade-transition>
                  </v-card>
                </template>
              </v-hover>
            </v-col>
          </v-row>
        </v-card-text>
      </v-card>
    </v-row>
    <v-dialog
      v-model="settingDialog"
      width="600">
      <v-card>
        <v-card-text>
          <v-text-field
            v-model="name"
            label="NAME" />
          <v-text-field
            v-model="hobby"
            label="HOBBY" />
          <v-text-field
            v-model="skills"
            label="SKILLS" />
          <v-textarea
            v-model="profile"
            label="PROFILE" />
          <v-combobox
            v-model="strength"
            :items="strength"
            hide-selected
            label="STRENGTH"
            multiple
            chips />
        </v-card-text>
      </v-card>
    </v-dialog>
  </div>
</template>
<script>
export default {
  data () {
    return {
      fab: false,
      settingDialog: false,
      color: "green lighten-2",
      colorList: [
        "green lighten-2",
        "blue lighten-2",
        "red lighten-2",
        "purple lighten-2",
        "yellow darken-2",
        "brown lighten-2",
        "grey darken-4"
      ],
      name: "山田 太郎",
      hobby: "#プログラミング",
      skills: [
        { item: "Ruby", rating: 3 },
        { item: "Ruby on Rails", rating: 4 },
        { item: "Vue.js", rating: 2 },
        { item: "Nuxt.js", rating: 2 },
      ],
      profile: "・要件定義からリリース、運用保守まで様々な工程での開発経験があります",
      strength: [
        "達成欲",
      ],
      experience: [
        { period: "2021.01 - 現在", project: "○○システム開発", content: "機能開発、運用保守を行いました。" },
      ],
    }
  },
  methods: {
    captureImage () {
      this.$html2canvas(document.querySelector('#capture')).then((canvas) => {
        const link = document.createElement('a')
        link.href = canvas.toDataURL()
        link.download = `resume.png`
        link.click()
      })
    },
    changeColor(color) {
      this.color = color;
    }
  }
}
</script>