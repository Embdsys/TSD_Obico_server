<template>
  <div>
    <h2 class="text-center">Printer Feed Sharing</h2>
    <hr />
    <div v-if="!isProAccount">
      <h5 class="mb-5">Wait! You need to <a href="/ent_pub/pricing/">upgrade to the Pro plan</a> to enable Printer feed. </h5>
      <p>Printer feed sharing is a Pro feature.</p>
      <p><a
          href="https://www.thespaghettidetective.com/docs/upgrade-to-pro#why-cant-the-detective-just-work-for-free-people-love-free-you-know">Running
          TSD incurs non-trivial amount of costs</a>. With little more than 1 Starbucks per month, you can upgrade to a
        Pro account and help us run TSD smoothly.</p>
      <p><a href="/ent_pub/pricing/">Check out Pro pricing >>></a></p>
    </div>
    <div v-else>
      <div class="py-3">
        <div class="custom-control custom-switch">
            <input
              type="checkbox"
              class="custom-control-input update-printer"
              :id="'sharing_enabled-toggle-' + printer.id"
              @click="onSharingToggled"
              :checked="sharedResource"
            >
            <label
              class="custom-control-label"
              :for="'sharing_enabled-toggle-' + printer.id"
              style="font-size: 1rem;"
            >Share live feed for printer "<b>{{ printer.name }}</b>"</label>
        </div>
        <div class="form-group">
          <div v-show="sharedLink">
            <div class="input-group mt-4 mb-2">
              <input
                type="text"
                id="secret-token-input"
                class="form-control shared-link-text"
                aria-label="Secret token"
                readonly
                :value="sharedLink"
                ref="sharedLink"
              >
              <div class="input-group-append">
                <div
                  id="copy-link"
                  class="copy-button"
                  data-clipboard-target="#secret-token-input"
                  aria-label="Copy secure link to clipboard"
                  @click="copyToClipboard"
                >
                  <i class="fas fa-clipboard"></i>
                </div>
                <b-tooltip :show.sync="copyStatus" target="copy-link" triggers="click" placement="bottom">{{ copyMessage }}</b-tooltip>
              </div>
            </div>
            <div class="my-1">Click the clipboard icon above to copy the secure shareable link to your clipboard.</div>
            <div class="my-1">You can test the shareable link by right-clicking <a :href="sharedLink">here</a>
              and select "Open Link in Incognito Window".</div>
          <br />
          <em class="text-muted">
            <small>
              <div>Notes:</div>
              <ul>
                <li>Send the secure link to anyone you want to share your printer feed with. They do NOT need a The
                  Spaghetti Detective account to see your printer feed.</li>
                <li>Anyone with this shareable link will be able to see your printer feed. <a href="https://www.thespaghettidetective.com/docs/printer-feed-sharing/">Learn more about what
                    they can see.</a></li>
              </ul>
            </small>
          </em>
          </div>
        </div>
        <br />
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import urls from '@config/server-urls'

export default {
  props: {
    isProAccount: {
      type: Boolean,
      default: false,
    },
    printer: {
      type: Object,
      required: true,
    },
  },

  data() {
    return {
      share: false,
      sharedResource: null,
      copyStatus: false,
      copyMessage: '',
    }
  },

  created() {
    this.fetchSharedResources()
  },

  computed: {
    sharedLink() {
      if (this.sharedResource) {
        const url = window.location
        return `${url.protocol}//${url.host}/printers/share_token/${this.sharedResource.share_token}/`
      }

      return ''
    }
  },

  methods: {
    fetchSharedResources() {
      return axios
        .get(urls.sharedResources({'printer_id': this.printer.id}))
        .then(response => {
          if (response.data.length > 0) {
            this.sharedResource = response.data[0]
          }
        })
    },
    postSharedResources() {
      return axios
        .post(urls.sharedResources({'printer_id': this.printer.id}))
        .then(response => {
          if (response.data.length > 0) {
            this.sharedResource = response.data[0]
          }
        })
    },
    deleteSharedResources() {
      return axios
        .delete(urls.sharedResource(this.sharedResource.id))
        .then(() => {
          this.sharedResource = null
        })
    },
    onSharingToggled() {
      if (this.sharedResource) {
        this.deleteSharedResources()
      } else {
        this.postSharedResources()
      }
    },
    // Copy share link to clipboard
    copyToClipboard() {
      this.copyStatus = true

      this.$refs.sharedLink.focus()
      this.$refs.sharedLink.select()
      try {
        document.execCommand('copy')
        this.copyMessage = 'Copied!'
      } catch (err) {
        console.error('Fallback: Oops, unable to copy', err)
        this.copyMessage = 'Failed!'
      }
    },
  }
}
</script>

<style lang="sass" scoped>
hr
  background-color: var(--color-divider)

.shared-link-text
  color: var(--text-primary) !important
  height: 40px
  border-color: var(--color-divider)
  background-color: var(--color-input-background) !important

.copy-button
  color: var(--color-primary)
  border: 2px solid var(--color-primary)
  background-color: transparent
  display: inline-flex
  align-items: center
  justify-content: center
  padding: 0 .75rem
  font-size: 1rem
  line-height: 1.5

  &:hover
    background-color: var(--color-primary)
    color: var(--color-text-primary)
    cursor: pointer
</style>
