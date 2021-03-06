<template>
  <div v-if="user">
    <KFormContainer>
      <QCardSection>
        <div class="text-h6">
          {{ $t('USERDATA.PROFILE_TITLE') }}
        </div>
      </QCardSection>
      <QCardSection>
        <ChangePhoto
          :value="user"
          :status="profileEditStatus"
          :label="$t('USERDATA.PHOTO')"
          :hint="$t('USERDATA.SET_PHOTO')"
          mime-type="image/jpeg"
          @save="saveUser({ photo: arguments[0] })"
        />
        <QSeparator />
        <ProfileEdit
          :value="user"
          :status="profileEditStatus"
          @save="saveUser"
        />
      </QCardSection>
    </KFormContainer>
    <KFormContainer>
      <QCardSection>
        <div class="text-h6">
          {{ $t('LANGUAGECHOOSER.SWITCH') }}
        </div>
      </QCardSection>
      <QCardSection>
        <div class="edit-box row justify-end">
          <LocaleSelect />
        </div>
      </QCardSection>
    </KFormContainer>
    <KFormContainer>
      <QCardSection>
        <div class="text-h6">
          {{ $t('USERDATA.ACCOUNT') }}
        </div>
      </QCardSection>
      <QCardSection>
        <ChangeEmail
          :user="user"
          :status="changeEmailStatus"
          @save="changeEmail"
        />
        <QSeparator />
        <ChangePassword
          :status="changePasswordStatus"
          @save="changePassword"
        />
        <QCardActions>
          <RequestDeleteAccount
            :status="requestDeleteAccountStatus"
            @requestDeleteAccount="requestDeleteAccount"
          />
        </QCardActions>
      </QCardSection>
    </KFormContainer>
    <GroupSettings />
    <KFormContainer
      v-if="!$q.platform.is.cordova"
    >
      <QCardSection>
        <div class="text-h6">
          {{ $t('USERDATA.PUSH') }}
        </div>
      </QCardSection>
      <QCardSection>
        <Push
          :value="pushEnabled"
          :pending="pushPending"
          @enable="enablePush"
          @disable="disablePush"
        />
      </QCardSection>
    </KFormContainer>
  </div>
</template>

<script>
import {
  QCardSection,
  QSeparator,
  QCardActions,
} from 'quasar'

import { mapGetters, mapActions } from 'vuex'

import ProfileEdit from '@/authuser/components/Settings/ProfileEdit'
import ChangePassword from '@/authuser/components/Settings/ChangePassword'
import ChangeEmail from '@/authuser/components/Settings/ChangeEmail'
import ChangePhoto from '@/authuser/components/Settings/ChangePhoto'
import RequestDeleteAccount from '@/authuser/components/Settings/RequestDeleteAccount'
import Push from '@/authuser/components/Settings/Push'
import LocaleSelect from '@/utils/components/LocaleSelect'
import GroupSettings from '@/group/pages/Settings'
import KFormContainer from '@/base/components/KFormContainer'

export default {
  name: 'Settings',
  components: {
    QCardSection,
    QSeparator,
    QCardActions,
    ProfileEdit,
    ChangePassword,
    ChangeEmail,
    ChangePhoto,
    RequestDeleteAccount,
    Push,
    LocaleSelect,
    GroupSettings,
    KFormContainer,
  },
  computed: {
    ...mapGetters({
      user: 'auth/user',
      profileEditStatus: 'auth/saveStatus',
      changePasswordStatus: 'auth/changePasswordStatus',
      changeEmailStatus: 'auth/changeEmailStatus',
      requestDeleteAccountStatus: 'users/requestDeleteAccountStatus',
      pushEnabled: 'auth/push/enabled',
      pushPending: 'auth/push/pending',
    }),
  },
  methods: {
    ...mapActions({
      saveUser: 'auth/save',
      changeEmail: 'auth/changeEmail',
      changePassword: 'auth/changePassword',
      requestDeleteAccount: 'users/requestDeleteAccount',
      enablePush: 'auth/push/enable',
      disablePush: 'auth/push/disable',
    }),
  },
}
</script>

<style scoped lang="stylus">
@import '~editbox'

</style>
