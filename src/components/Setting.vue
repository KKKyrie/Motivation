<template>
  <vue-modal @before-open='beforeOpen' :adaptive='true' name='noteModal'>
    <div class='pure-g'>
      <div class='pure-u-1-3'></div>
      <div class='pure-u-1-3'>
        <h2 class='title'>{{ $t('title.motivation') }}</h2>
      </div>
      <div class='pure-u-1-3'></div>
    </div>

    <!-- input area start -->
    <div class='pure-g'>

      <!-- year / month / date -->
      <div class='pure-u-1-2'>
        <form class='pure-form pure-form-stacked'>
          <fieldset class='pure-group'>
            <label for='year' class='pure-input-1 color'>{{ $t('title.date') }}</label>
            <input
              id='year'
              class='pure-input-1'
              v-model='year'
              type='number'
              :placeholder='$t("placeholder.year")'
            >
            <input
              v-model='month'
              class='pure-input-1'
              type='number'
              :placeholder='$t("placeholder.month")'
            >
            <input
              v-model='day'
              class='pure-input-1'
              type='number'
              :placeholder='$t("placeholder.day")'
            >
          </fieldset>
        </form>
      </div>
      <!-- year / month / date -->

      <!-- level -->
      <div class='pure-u-1-2'>
        <form class='pure-form pure-form-stacked'>
          <fieldset class='pure-group'>
            <label class='color' for='level'>{{ $t('title.level') }}</label>
            <select id='level' v-model='level' class='pure-input-1'>
              <option :value='`light`'>{{ $t('level.light') }}</option>
              <option :value='`moderate`'>{{ $t('level.moderate') }}</option>
              <option :value='`terrifying`'>{{ $t('level.terrifying') }}</option>
              <option :value='`wtf`'>{{ $t('level.wtf') }}</option>
            </select>
          </fieldset>
        </form>
      </div>
      <!-- level -->

      <span class='button-container'>
        <button type='button' @click='update' class='pure-button confirm-button'>{{ $t('button.confirm') }}</button>
        <span class='read-me'>{{ $t('message.readme') }}</span>
        <span class='read-me sub-info'>Made by <a class='no-decoration' href='https://kyrieliu.cn'>@kyrieliu</a></span>
        <span class='read-me sub-info'>Powered by <a class='no-decoration' href='https://github.com/NeverBehave/Motivation'>NeverBehave</a></span>
      </span>
    </div>
    <!-- input area end -->

  </vue-modal>
</template>

<script>
export default {
  name: 'Setting',
  data () {
    return {
      year: null,
      month: null,
      day: null
    }
  },
  computed: {
    time: function () {
      return this.$store.getters.getDate
    },
    level: {
      get () {
        return this.$store.state.level
      },
      set (value) {
        this.$store.commit('updateLevel', value)
      }
    }
  },
  methods: {
    pad (d) {
      return d.toString().padStart(2, '0')
    },
    beforeOpen () {
      if (this.time != null) {
        this.year = this.time.year()
        this.month = this.time.month() + 1 // 0 - 11, as API said
        this.day = this.time.date()
      }
    },
    update () {
      let str =
        this.year + '-' + this.pad(this.month) + '-' + this.pad(this.day)
      this.$store.dispatch('updateTime', str).then(r => {
        if (!r) {
          this.$toasted.error(this.$t('toasted.date_invaild'))
          return false
        }

        this.$toasted.info(this.$t('toasted.set_date'))
        this.close()
      })
    },
    close () {
      this.$modal.hide('noteModal')
    }
  }
}
</script>

<style>
.color {
  color: white;
}

.read-me {
  display: block;
  width: 100%;
  margin-top: 20px;
  color: #cdcdcd;
  letter-spacing: 1px;
}

.sub-info {
  margin-top: 10px;
  font-size: 14px;
}

.no-decoration {
  text-decoration: none;
}

.title {
  color: white;
  text-align: center;
}

.pure-form-stacked {
  margin: 0 10px 10px 0;
}

.v--modal {
  background-color: #151513 !important;
  top: 50% !important;
  height: 400px !important;
  padding: 0 10px !important;
  transform: translateY(-50%);
}

.confirm-button-area {
  border: 1px solid white;
  width: 100%;
  height: 50px;
}

.button-container {
  text-align: center;
  width: 100%;
}

.confirm-button {
  width: 80%;
}
</style>
