<template>
  <q-dialog ref="dialog" persistent>
    <q-card style="max-height: 90vh;min-height: 70vh;min-width: 70vw">
      <q-toolbar>
        <q-avatar>
          <q-icon
            name="settings"
            class="text-primary"
            style="font-size: 1.8em"
          />
        </q-avatar>
        <q-toolbar-title>
          <span class="text-weight-bold">{{ $t('settings') }}</span>
        </q-toolbar-title>
        <q-btn flat round dense icon="close" v-close-popup />
      </q-toolbar>

      <q-card-section class="scroll">
        <q-splitter v-model="splitterModel" style="min-height: 40vh">
          <template v-slot:before>
            <q-tabs v-model="tab" vertical class="text-teal no-border">
              <q-tab
                name="general"
                icon="tune"
                :label="$t('general')"
                class="text-primary"
              />
              <q-tab
                name="editor"
                icon="edit_attributes"
                :label="$t('editor')"
                class="text-amber-10"
              />
              <q-tab
                name="server"
                icon="storage"
                :label="$t('server')"
                class="text-red-7"
              />
            </q-tabs>
          </template>

          <template v-slot:after>
            <q-tab-panels
              v-model="tab"
              animated
              swipeable
              vertical
              transition-prev="jump-up"
              transition-next="jump-up"
            >
              <q-tab-panel name="general">
                <div class="text-h4 q-mb-md">{{ $t('general') }}</div>
                <q-separator />
                <div>
                  <div class="text-h6 q-mb-md setting-item">
                    {{ $t('language') }}
                  </div>
                  <q-select
                    :value="$t(language)"
                    :options="languageOptions"
                    @input="languageChangeHandler"
                  />
                </div>
                <div>
                  <div class="text-h6 q-mb-md setting-item">
                    <span>{{ $t('darkMode') }}</span>
                    <q-toggle :value="darkMode" color="black" @input="(v) => toggleDarkMode(v)" />
                  </div>
                </div>
              </q-tab-panel>

              <q-tab-panel name="editor">
                <div class="text-h4 q-mb-md">{{ $t('editor') }}</div>
                <q-separator />
              </q-tab-panel>

              <q-tab-panel name="server">
                <div class="text-h4 q-mb-md">{{ $t('server') }}</div>
                <q-separator />
                <div>
                  <div class="text-h6 q-mb-md setting-item">
                    <span>{{ $t('markdownOnly') }}</span>
                    <q-toggle :value="markdownOnly" color="primary" @input="(v) => toggleMarkdownOnly(v)" />
                  </div>
                </div>
              </q-tab-panel>
            </q-tab-panels>
          </template>
        </q-splitter>
      </q-card-section>
    </q-card>
  </q-dialog>
</template>

<script>
import { createNamespacedHelpers } from 'vuex'
import { i18n } from '../../boot/i18n'
const { mapState, mapActions } = createNamespacedHelpers('client')

export default {
  name: 'SettingsDialog',
  data () {
    return {
      tab: 'general',
      splitterModel: 20
    }
  },
  computed: {
    ...mapState(['language', 'darkMode', 'markdownOnly']),
    languageOptions: function () {
      return i18n.availableLocales.map(l => i18n.t(l))
    }
  },
  methods: {
    toggle: function () {
      return this.$refs.dialog.toggle()
    },
    languageChangeHandler: function (lan) {
      lan = i18n.availableLocales.find(l => {
        return i18n.t(l) === lan
      })
      this.setLanguage(lan)
      i18n.locale = lan
    },
    ...mapActions(['setLanguage', 'toggleDarkMode', 'toggleMarkdownOnly'])
  }
}
</script>

<style scoped>
.setting-item {
  margin-top: 2rem;
}
</style>
