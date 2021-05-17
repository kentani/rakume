<template>
  <div>
    <v-row justify="center" class="mb-6">
      <v-col cols="12" md="12" lg="8" xl="6">
        <v-card tile elevation="8">
          <v-card-actions class="pb-0">
            <span class="body-2 font-weight-bold ml-1 mr-2 pt-1">一括設定</span>
            <v-file-input
              hide-details
              class="mt-0 mb-2"
              label="jsonファイル取り込み"
              :color="color"
              :disabled="isPreview"
              @change="getFileContent"
            />
            <v-btn
              rounded
              depressed
              class="mx-2"
              :dark="!isPreview && !!json"
              :color="color"
              :disabled="isPreview || !json"
              :ripple="false"
              @click="importJson"
            >
              反映する
            </v-btn>
          </v-card-actions>
          <v-card-actions>
            <span class="body-2 font-weight-bold ml-1 mr-2 pt-1">プレビュー</span>
            <v-switch
              v-model="isPreview"
              inset
              hide-details
              class="ma-2"
              :color="color"
            />
            <v-spacer />
            <span class="body-2 font-weight-bold ml-1 mr-2 pt-1">ダウンロード</span>
            <v-btn
              rounded
              depressed
              class="mx-2"
              :dark="!isPreview"
              :color="color"
              :disabled="isPreview"
              :ripple="false"
              @click="downloadJson"
            >
              設定ファイル
            </v-btn>
            <v-btn
              rounded
              depressed
              class="mx-2"
              :dark="isPreview"
              :color="color"
              :disabled="!isPreview"
              :ripple="false"
              @click="captureImage"
            >
              RESUME
            </v-btn>
          </v-card-actions>
          <v-card-actions>
            <span class="body-2 font-weight-bold ml-1 mr-2">テーマ</span>
            <v-btn
              v-for="colorName in colorList" :key="colorName"
              fab
              x-small
              depressed
              :ripple="false"
              :color="colorName"
              @click="changeColor(colorName)"
            >
              <v-icon v-if="color == colorName" color="white">mdi-check</v-icon>
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
    <v-row justify="center">
      <v-col cols="12" md="12" lg="8" xl="6">
        <v-card tile id="capture" :color="color" elevation="8">
          <v-card-text>
            <v-row>
              <v-col cols="12" sm="5" style="background-color: #EEEEEE; min-height: 100%;">
                <v-hover>
                  <template v-slot:default="{ hover }">
                    <v-card v-if="name" flat class="mb-6" style="background-color: #EEEEEE; border: 2px solid; border-radius: 50px;">
                      <v-card-text>
                        <h2 class="display-1 grey--text text--darken-2 text-center font-weight-bold mb-0">{{ name }}</h2>
                      </v-card-text>
                      <v-fade-transition>
                        <v-overlay
                          v-if="!isPreview && hover"
                          absolute
                          opacity="0.2"
                        >
                          <v-btn rounded depressed class="mx-2" @click="openNameModal">編集する</v-btn>
                          <v-btn rounded depressed class="mx-2" @click="deleteName">削除する</v-btn>
                        </v-overlay>
                      </v-fade-transition>
                    </v-card>
                    <v-card v-else flat class="mb-6" style="background-color: #EEEEEE; border: 2px solid; border-radius: 50px;">
                      <v-card-text>
                        <v-row justify="center" class="body-2 grey--text text--darken-1 pa-4">
                          <v-btn v-if="!isPreview" rounded depressed dark :color="color" @click="openNameModal">＋名前を追加する</v-btn>
                        </v-row>
                      </v-card-text>
                    </v-card>
                  </template>
                </v-hover>
                <v-dialog
                  v-model="nameModal"
                  width="600"
                >
                  <edit-name-modal
                    :name="this.name"
                    :color="color"
                    @handleEditNameClosed="editNameClosed"
                    @handleEditNameSaved="editNameSaved"
                  />
                </v-dialog>
                <h3 class="title grey--text text--darken-2 font-weight-bold mb-0">HOBBY</h3>
                <v-hover>
                  <template v-slot:default="{ hover }">
                    <v-card v-if="hobby" tile flat style="background-color: #EEEEEE; white-space: pre-line;">
                      <v-card-text class="body-2 grey--text text--darken-1 pa-2">
                        <p class="mb-0">{{ hobby }}</p>
                      </v-card-text>
                      <v-fade-transition>
                        <v-overlay
                          v-if="!isPreview && hover"
                          absolute
                          opacity="0.2"
                        >
                          <v-btn rounded depressed class="mx-2" @click="openHobbyModal">編集する</v-btn>
                          <v-btn rounded depressed class="mx-2" @click="deleteHobby">削除する</v-btn>
                        </v-overlay>
                      </v-fade-transition>
                    </v-card>
                    <v-card v-else tile flat style="background-color: #EEEEEE;">
                      <v-card-text>
                        <v-row justify="center" class="body-2 grey--text text--darken-1 pa-4">
                          <v-btn v-if="!isPreview" rounded depressed dark :color="color" @click="openHobbyModal">＋趣味を追加する</v-btn>
                        </v-row>
                      </v-card-text>
                    </v-card>
                  </template>
                </v-hover>
                <v-dialog
                  v-model="hobbyModal"
                  width="600"
                >
                  <edit-hobby-modal
                    :hobby="hobby"
                    :color="color"
                    @handleEditHobbyClosed="editHobbyClosed"
                    @handleEditHobbySaved="editHobbySaved"
                  />
                </v-dialog>
                <v-divider class="mt-3 mb-6" style="border: 1px solid black; border-color: black;" />
                <h3 class="title grey--text text--darken-2 font-weight-bold mb-0">SKILLS</h3>
                <div
                  v-for="(skill, i) in skills"
                  :key="skill.item">
                <v-hover>
                  <template v-slot:default="{ hover }">
                    <v-card tile flat style="background-color: #EEEEEE;">
                      <v-card-actions class="body-2 grey--text text--darken-1">
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
                          v-if="!isPreview && hover"
                          absolute
                          opacity="0.2"
                        >
                          <v-btn rounded depressed class="mx-2" @click="openSkillModal(i)">編集する</v-btn>
                          <v-btn rounded depressed class="mx-2" @click="deleteSkill(i)">削除する</v-btn>
                        </v-overlay>
                      </v-fade-transition>
                    </v-card>
                  </template>
                </v-hover>
                </div>
                <v-dialog
                  v-model="skillModal"
                  width="600"
                >
                  <edit-skill-modal
                    :skill="skill"
                    :color="color"
                    @handleEditSkillClosed="editSkillClosed"
                    @handleEditSkillSaved="editSkillSaved"
                  />
                </v-dialog>
                <v-card v-if="!isPreview" tile flat style="background-color: #EEEEEE;">
                  <v-card-text>
                    <v-row justify="center" class="body-2 grey--text text--darken-1 pa-4">
                      <v-btn rounded depressed dark :color="color" @click="openSkillModal(skills.length + 1)">＋スキルを追加する</v-btn>
                    </v-row>
                  </v-card-text>
                </v-card>
              </v-col>
              <v-col cols="12" sm="7" style="min-height: 100%;">
                <h3 class="title font-weight-bold white--text mb-0">PROFILE</h3>
                <v-hover>
                  <template v-slot:default="{ hover }">
                    <v-card v-if="profile" tile flat :color="color" style="white-space: pre-line;">
                      <v-card-text class="body-2 white--text pa-2">
                        <p class="mb-0">{{ profile }}</p>
                      </v-card-text>
                      <v-fade-transition>
                        <v-overlay
                          v-if="!isPreview && hover"
                          absolute
                          opacity="0.2"
                        >
                          <v-btn rounded depressed class="mx-2" @click="openProfileModal">編集する</v-btn>
                          <v-btn rounded depressed class="mx-2" @click="deleteProfile">削除する</v-btn>
                        </v-overlay>
                      </v-fade-transition>
                    </v-card>
                    <v-card v-else tile flat :color="color" style="white-space: pre-line;">
                      <v-card-text>
                        <v-row justify="center" class="body-2 grey--text text--darken-1 pa-4">
                          <v-btn v-if="!isPreview" rounded depressed color="grey lighten-3" @click="openProfileModal">＋プロフィールを追加する</v-btn>
                        </v-row>
                      </v-card-text>
                    </v-card>
                  </template>
                </v-hover>
                <v-dialog
                  v-model="profileModal"
                  width="600"
                >
                  <edit-profile-modal
                    :profile="profile"
                    :color="color"
                    @handleEditProfileClosed="editProfileClosed"
                    @handleEditProfileSaved="editProfileSaved"
                  />
                </v-dialog>
                <v-divider class="mt-3 mb-6" style="border: 1px solid white; border-color: white;" />
                <h3 class="title font-weight-bold white--text mb-0">STRENGTH</h3>
                <v-hover>
                  <template v-slot:default="{ hover }">
                    <v-card v-if="strength.length > 0" tile flat :color="color" style="white-space: pre-line;">
                      <v-card-text class="body-2 white--text pa-2">
                        <v-chip v-for="(strength, i) in strength" :key="i" class="mx-1" color="grey lighten-3">
                          <span class="body-2 grey--text text--darken-2">{{ i + 1 }}. {{ strength }}</span>
                        </v-chip>
                      </v-card-text>
                      <v-fade-transition>
                        <v-overlay
                          v-if="!isPreview && hover"
                          absolute
                          opacity="0.2">
                          <v-btn rounded depressed class="mx-2" @click="openStrengthModal">編集する</v-btn>
                          <v-btn rounded depressed class="mx-2" @click="deleteStrength">削除する</v-btn>
                        </v-overlay>
                      </v-fade-transition>
                    </v-card>
                    <v-card v-else tile flat :color="color" style="white-space: pre-line;">
                      <v-card-text>
                        <v-row justify="center" class="body-2 grey--text text--darken-1 pa-4">
                          <v-btn v-if="!isPreview" rounded depressed color="grey lighten-3" @click="openStrengthModal">＋強みを追加する</v-btn>
                        </v-row>
                      </v-card-text>
                    </v-card>
                  </template>
                </v-hover>
                <v-dialog
                  v-model="strengthModal"
                  width="600"
                >
                  <edit-strength-modal
                    :strengthList="strength"
                    :color="color"
                    @handleEditStrengthClosed="editStrengthClosed"
                    @handleEditStrengthSaved="editStrengthSaved"
                  />
                </v-dialog>
                <v-divider class="mt-3 mb-6" style="border: 1px solid white; border-color: white;" />
                <h3 class="title font-weight-bold white--text mb-0">EXPERIENCE</h3>
                <div
                  v-for="(experience, i) in experiences"
                  :key="i"
                >
                  <v-hover>
                    <template v-slot:default="{ hover }">
                      <v-card tile flat :color="color" style="white-space: pre-line;">
                        <v-card-text class="body-2 white--text pa-2">
                          <v-timeline dense>
                            <v-timeline-item
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
                            v-if="!isPreview && hover"
                            absolute
                            opacity="0.2">
                            <v-btn rounded depressed class="mx-2" @click="openExperienceModal(i)">編集する</v-btn>
                            <v-btn rounded depressed class="mx-2" @click="deleteExperience(i)">削除する</v-btn>
                          </v-overlay>
                        </v-fade-transition>
                      </v-card>
                    </template>
                  </v-hover>
                </div>
                <v-dialog
                  v-model="experienceModal"
                  width="600">
                  <edit-experience-modal
                    :experience="experience"
                    :color="color"
                    @handleEditExperienceClosed="editExperienceClosed"
                    @handleEditExperienceSaved="editExperienceSaved"
                  />
                </v-dialog>
                <v-card v-if="!isPreview" tile flat :color="color">
                  <v-card-text>
                    <v-row justify="center" class="body-2 grey--text text--darken-1 pa-4">
                      <v-btn rounded depressed color="grey lighten-3" @click="openExperienceModal(experiences.length + 1)">＋経歴を追加する</v-btn>
                    </v-row>
                  </v-card-text>
                </v-card>
              </v-col>
            </v-row>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </div>
</template>

<script>
import editSkillModal from '../components/editSkillModal.vue'
import { saveAs } from "file-saver"

export default {
  components: { editSkillModal },
  data () {
    return {
      isPreview: false,
      nameModal: false,
      hobbyModal: false,
      skillModal: false,
      profileModal: false,
      strengthModal: false,
      experienceModal: false,
      previewModal: false,
      json: null,
      name: "",
      hobby: "",
      skills: [],
      profile: "",
      strength: [],
      experiences: [],
      skill: {},
      skillIndex: null,
      experience: {},
      experienceIndex: null,
      color: "green lighten-2",
      colorList: [
        "green lighten-2",
        "blue lighten-2",
        "red lighten-2",
        "purple lighten-2",
        "yellow darken-2",
        "brown lighten-2",
        "grey darken-2",
        "grey darken-4"
      ],
    }
  },
  watch: {
    isPreview: function(val) {
      const el = document.querySelector('#capture');
      if (val) {
        el.style.minWidth = '1000px'
      } else {
        el.style.minWidth = ''
      }
    }
  },
  methods: {
    async getFileContent (file) {
      if (!file) {
        this.json = null
        return
      }

      if (!this.checkFile(file)) {
        alert("ファイルを読み込めませんでした")
        return
      }

      try {
        const body = await this.readFileAsync(file)
        this.json = JSON.parse(body)
      } catch (e) {
        console.log(e)
      }
    },
    readFileAsync (file) {
      return new Promise((resolve, reject) => {
        const reader = new FileReader()
        reader.onload = () => {
          resolve(reader.result)
        }
        reader.onerror = reject
        reader.readAsText(file)
      })
    },
    checkFile(file) {
      if (file.type !== 'application/json') {
        return false
      }

      const SIZE_LIMIT = 5000000 // 5MB
      if (file.size > SIZE_LIMIT) {
        return false
      }
      return true
    },
    importJson() {
      this.name = this.json.name
      this.hobby = this.json.hobby
      this.skills = this.json.skills
      this.profile = this.json.profile
      this.strength = this.json.strength
      this.experiences = this.json.experiences
    },
    downloadJson() {
      const resume = {
        "name": this.name,
        "hobby": this.hobby,
        "skills": this.skills,
        "profile": this.profile,
        "strength": this.strength,
        "experiences": this.experiences,
      }
      const blob = new Blob([JSON.stringify(resume)], {
        type: "application/json"
      })

      saveAs(blob, "resume.json")
    },
    async captureImage () {
      const el = document.querySelector('#capture');
      this.$html2canvas(el).then((canvas) => {
        const link = document.createElement('a')
        link.href = canvas.toDataURL()
        link.download = "resume.png"
        link.click()
      })
    },
    changeColor(color) {
      this.color = color;
    },
    setColorText(color) {
      const [ main, sub ] = color.split(" ")
      return `${main}--text text--${sub}`
    },
    openNameModal() {
      this.nameModal = true
    },
    editNameClosed() {
      this.nameModal = false
    },
    editNameSaved(newName) {
      this.name = newName
      this.nameModal = false
    },
    deleteName() {
      this.name = ""
    },
    openHobbyModal() {
      this.hobbyModal = true
    },
    editHobbyClosed() {
      this.hobbyModal = false
    },
    editHobbySaved(newHobby) {
      this.hobby = newHobby
      this.hobbyModal = false
    },
    deleteHobby() {
      this.hobby = ""
    },
    openSkillModal(i) {
      this.skillIndex = i
      this.skill = Object.assign({}, this.skills[i])
      this.skillModal = true
    },
    editSkillClosed() {
      this.skillModal = false
    },
    editSkillSaved(newSkill) {
      this.skill = newSkill
      this.skills.splice(this.skillIndex, 1, newSkill)
      this.skillModal = false
    },
    deleteSkill(i) {
      this.skills.splice(i, 1)
    },
    openProfileModal() {
      this.profileModal = true
    },
    editProfileClosed() {
      this.profileModal = false
    },
    editProfileSaved(newProfile) {
      this.profile = newProfile
      this.profileModal = false
    },
    deleteProfile() {
      this.profile = ""
    },
    openStrengthModal() {
      this.tmpStrength = this.strength
      this.strengthModal = true
    },
    editStrengthClosed() {
      this.strengthModal = false
    },
    editStrengthSaved(newStrength) {
      this.strength = newStrength
      this.strengthModal = false
    },
    deleteStrength() {
      this.strength = []
    },
    openExperienceModal(i) {
      this.experienceIndex = i
      this.experience = Object.assign({}, this.experiences[i])
      this.experienceModal = true
    },
    editExperienceClosed() {
      this.experienceModal = false
    },
    editExperienceSaved(newExperience) {
      this.experience = newExperience
      this.experiences.splice(this.experienceIndex, 1, newExperience)
      this.experienceModal = false
    },
    deleteExperience(i) {
      this.experiences.splice(i, 1)
    }
  }
}
</script>