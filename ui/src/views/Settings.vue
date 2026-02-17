<!--
  Copyright (C) 2022 Nethesis S.r.l.
  SPDX-License-Identifier: GPL-3.0-or-later
-->
<template>
  <cv-grid fullWidth>
    <cv-row>
      <cv-column class="page-title">
        <h2>{{ $t("settings.title") }}</h2>
      </cv-column>
    </cv-row>
    <cv-row v-if="error.getConfiguration">
      <cv-column>
        <NsInlineNotification
          kind="error"
          :title="$t('action.get-configuration')"
          :description="error.getConfiguration"
          :showCloseButton="false"
        />
      </cv-column>
    </cv-row>
    <cv-row>
      <cv-column>
        <cv-tile light>
          <cv-form @submit.prevent="configureModule">
            <cv-text-input
              :label="$t('settings.postiz_fqdn')"
              placeholder="postiz.example.org"
              v-model.trim="host"
              class="mg-bottom"
              :invalid-message="$t(error.host)"
              :disabled="loading.getConfiguration || loading.configureModule"
              ref="host"
            >
            </cv-text-input>
            <cv-text-input
              :label="$t('settings.temporal_host')"
              placeholder="temporal.example.org"
              v-model.trim="temporal_host"
              class="mg-bottom"
              :invalid-message="$t(error.temporal_host)"
              :disabled="loading.getConfiguration || loading.configureModule"
              ref="temporal_host"
            >
            </cv-text-input>
            <cv-toggle
              value="letsEncrypt"
              :label="$t('settings.lets_encrypt')"
              v-model="isLetsEncryptEnabled"
              :disabled="loading.getConfiguration || loading.configureModule"
              class="mg-bottom"
            >
              <template slot="text-left">{{
                $t("settings.disabled")
              }}</template>
              <template slot="text-right">{{
                $t("settings.enabled")
              }}</template>
            </cv-toggle>
            <cv-toggle
              value="httpToHttps"
              :label="$t('settings.http_to_https')"
              v-model="isHttpToHttpsEnabled"
              :disabled="loading.getConfiguration || loading.configureModule"
              class="mg-bottom"
            >
              <template slot="text-left">{{
                $t("settings.disabled")
              }}</template>
              <template slot="text-right">{{
                $t("settings.enabled")
              }}</template>
            </cv-toggle>
            <!-- advanced options -->
            <cv-accordion ref="accordion" class="maxwidth mg-bottom">
              <cv-accordion-item :open="toggleAccordion[0]">
                <template slot="title">{{ $t("settings.advanced") }}</template>
                <template slot="content">
                  <cv-text-input
                    :label="$t('settings.is_general')"
                    v-model="is_general"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.next_public_upload_directory')"
                    v-model="next_public_upload_directory"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.github_client_secret')"
                    v-model="github_client_secret"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.beehiive_api_key')"
                    v-model="beehiive_api_key"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.youtube_client_id')"
                    v-model="youtube_client_id"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.dribbble_client_id')"
                    v-model="dribbble_client_id"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.facebook_app_secret')"
                    v-model="facebook_app_secret"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.storage_provider')"
                    v-model="storage_provider"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.x_api_key')"
                    v-model="x_api_key"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.threads_app_secret')"
                    v-model="threads_app_secret"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.tiktok_client_secret')"
                    v-model="tiktok_client_secret"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.slack_secret')"
                    v-model="slack_secret"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.linkedin_client_secret')"
                    v-model="linkedin_client_secret"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.slack_signing_secret')"
                    v-model="slack_signing_secret"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.mastodon_url')"
                    v-model="mastodon_url"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.mastodon_client_secret')"
                    v-model="mastodon_client_secret"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.next_public_polotno')"
                    v-model="next_public_polotno"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.stripe_signing_key')"
                    v-model="stripe_signing_key"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.beehiive_publication_id')"
                    v-model="beehiive_publication_id"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.pinterest_client_id')"
                    v-model="pinterest_client_id"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.dribbble_client_secret')"
                    v-model="dribbble_client_secret"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.slack_id')"
                    v-model="slack_id"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.next_public_discord_support')"
                    v-model="next_public_discord_support"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.github_client_id')"
                    v-model="github_client_id"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.discord_client_secret')"
                    v-model="discord_client_secret"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.openai_api_key')"
                    v-model="openai_api_key"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.disable_registration')"
                    v-model="disable_registration"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.run_cron')"
                    v-model="run_cron"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.reddit_client_secret')"
                    v-model="reddit_client_secret"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.fee_amount')"
                    v-model="fee_amount"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.nx_add_plugins')"
                    v-model="nx_add_plugins"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.linkedin_client_id')"
                    v-model="linkedin_client_id"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.threads_app_id')"
                    v-model="threads_app_id"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.facebook_app_id')"
                    v-model="facebook_app_id"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.youtube_client_secret')"
                    v-model="youtube_client_secret"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.tiktok_client_id')"
                    v-model="tiktok_client_id"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.pinterest_client_secret')"
                    v-model="pinterest_client_secret"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.mastodon_client_id')"
                    v-model="mastodon_client_id"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.x_api_secret')"
                    v-model="x_api_secret"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.reddit_client_id')"
                    v-model="reddit_client_id"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.discord_client_id')"
                    v-model="discord_client_id"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.discord_bot_token_id')"
                    v-model="discord_bot_token_id"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.api_limit')"
                    v-model="api_limit"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.stripe_publishable_key')"
                    v-model="stripe_publishable_key"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.stripe_secret_key')"
                    v-model="stripe_secret_key"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.stripe_signing_key_connect')"
                    v-model="stripe_signing_key_connect"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-select
                    :label="$t('settings.email_provider')"
                    v-model="email_provider"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                    <cv-select-option value="nodemailer"
                      >Nodemailer</cv-select-option
                    >
                    <cv-select-option value="smtp">SMTP</cv-select-option>
                    <cv-select-option value="resend">Resend</cv-select-option>
                  </cv-select>
                  <cv-text-input
                    :label="$t('settings.email_host')"
                    v-model="email_host"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.email_port')"
                    v-model="email_port"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.email_user')"
                    v-model="email_user"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.email_pass')"
                    v-model="email_pass"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.email_secure')"
                    v-model="email_secure"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.dub_token')"
                    v-model="dub_token"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.dub_api_endpoint')"
                    v-model="dub_api_endpoint"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.dub_short_link_domain')"
                    v-model="dub_short_link_domain"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.short_io_secret_key')"
                    v-model="short_io_secret_key"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.kutt_api_key')"
                    v-model="kutt_api_key"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.kutt_api_endpoint')"
                    v-model="kutt_api_endpoint"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.kutt_short_link_domain')"
                    v-model="kutt_short_link_domain"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.link_drip_api_key')"
                    v-model="link_drip_api_key"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.link_drip_api_endpoint')"
                    v-model="link_drip_api_endpoint"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.link_drip_short_link_domain')"
                    v-model="link_drip_short_link_domain"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.next_public_postiz_oauth_display_name')"
                    v-model="next_public_postiz_oauth_display_name"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.next_public_postiz_oauth_logo_url')"
                    v-model="next_public_postiz_oauth_logo_url"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-toggle
                    value="postizGenericOauth"
                    :label="$t('settings.postiz_generic_oauth')"
                    v-model="postiz_generic_oauth"
                    :disabled="loading.getConfiguration || loading.configureModule"
                    class="mg-bottom"
                  >
                    <template slot="text-left">{{
                      $t("settings.disabled")
                    }}</template>
                    <template slot="text-right">{{
                      $t("settings.enabled")
                    }}</template>
                  </cv-toggle>
                  <cv-text-input
                    :label="$t('settings.postiz_oauth_url')"
                    v-model="postiz_oauth_url"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.postiz_oauth_auth_url')"
                    v-model="postiz_oauth_auth_url"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.postiz_oauth_token_url')"
                    v-model="postiz_oauth_token_url"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.postiz_oauth_userinfo_url')"
                    v-model="postiz_oauth_userinfo_url"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.postiz_oauth_client_id')"
                    v-model="postiz_oauth_client_id"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.postiz_oauth_client_secret')"
                    v-model="postiz_oauth_client_secret"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.postiz_oauth_scope')"
                    v-model="postiz_oauth_scope"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.cloudflare_account_id')"
                    v-model="cloudflare_account_id"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.cloudflare_access_key')"
                    v-model="cloudflare_access_key"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.cloudflare_secret_access_key')"
                    v-model="cloudflare_secret_access_key"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.cloudflare_bucketname')"
                    v-model="cloudflare_bucketname"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.cloudflare_bucket_url')"
                    v-model="cloudflare_bucket_url"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.cloudflare_region')"
                    v-model="cloudflare_region"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.next_public_sentry_dsn')"
                    v-model="next_public_sentry_dsn"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                  <cv-text-input
                    :label="$t('settings.sentry_spotlight')"
                    v-model="sentry_spotlight"
                    class="mg-bottom"
                    :disabled="
                      loading.getConfiguration || loading.configureModule
                    "
                  >
                  </cv-text-input>
                </template>
              </cv-accordion-item>
            </cv-accordion>
            <cv-row v-if="error.configureModule">
              <cv-column>
                <NsInlineNotification
                  kind="error"
                  :title="$t('action.configure-module')"
                  :description="error.configureModule"
                  :showCloseButton="false"
                />
              </cv-column>
            </cv-row>
            <NsButton
              kind="primary"
              :icon="Save20"
              :loading="loading.configureModule"
              :disabled="loading.getConfiguration || loading.configureModule"
              >{{ $t("settings.save") }}</NsButton
            >
            <NsButton
              kind="secondary"
              :icon="Restart20"
              :loading="loading.restartPostiz"
              :disabled="loading.getConfiguration || loading.configureModule || loading.restartPostiz"
              @click="restartPostiz"
              class="mg-left"
              >{{ $t("settings.restart_postiz") }}</NsButton
            >
            <p class="restart-description">{{ $t("settings.restart_postiz_description") }}</p>
          </cv-form>
        </cv-tile>
      </cv-column>
    </cv-row>
  </cv-grid>
</template>

<script>
import to from "await-to-js";
import { mapState } from "vuex";
import {
  QueryParamService,
  UtilService,
  TaskService,
  IconService,
  PageTitleService,
} from "@nethserver/ns8-ui-lib";

export default {
  name: "Settings",
  mixins: [
    TaskService,
    IconService,
    UtilService,
    QueryParamService,
    PageTitleService,
  ],
  pageTitle() {
    return this.$t("settings.title") + " - " + this.appName;
  },
  data() {
    return {
      q: {
        page: "settings",
      },
      urlCheckInterval: null,
      host: "",
      temporal_host: "",
      isLetsEncryptEnabled: false,
      isHttpToHttpsEnabled: true,
      // Postiz settings
      is_general: "true",
      next_public_upload_directory: "/uploads",
      github_client_secret: "",
      beehiive_api_key: "",
      youtube_client_id: "",
      dribbble_client_id: "",
      facebook_app_secret: "",
      storage_provider: "local",
      x_api_key: "",
      threads_app_secret: "",
      tiktok_client_secret: "",
      slack_secret: "",
      linkedin_client_secret: "",
      slack_signing_secret: "",
      mastodon_url: "https://mastodon.social",
      mastodon_client_secret: "",
      next_public_polotno: "",
      stripe_signing_key: "",
      beehiive_publication_id: "",
      pinterest_client_id: "",
      dribbble_client_secret: "",
      slack_id: "",
      next_public_discord_support: "",
      github_client_id: "",
      discord_client_secret: "",
      openai_api_key: "",
      disable_registration: "false",
      run_cron: "true",
      reddit_client_secret: "",
      fee_amount: "0.05",
      nx_add_plugins: "false",
      linkedin_client_id: "",
      threads_app_id: "",
      facebook_app_id: "",
      youtube_client_secret: "",
      tiktok_client_id: "",
      pinterest_client_secret: "",
      mastodon_client_id: "",
      x_api_secret: "",
      reddit_client_id: "",
      discord_client_id: "",
      discord_bot_token_id: "",
      api_limit: "30",
      stripe_publishable_key: "",
      stripe_secret_key: "",
      stripe_signing_key_connect: "",
      email_provider: "nodemailer",
      email_host: "",
      email_port: "465",
      email_user: "",
      email_pass: "",
      email_secure: "true",
      dub_token: "",
      dub_api_endpoint: "",
      dub_short_link_domain: "",
      short_io_secret_key: "",
      kutt_api_key: "",
      kutt_api_endpoint: "",
      kutt_short_link_domain: "",
      link_drip_api_key: "",
      link_drip_api_endpoint: "",
      link_drip_short_link_domain: "",
      next_public_postiz_oauth_display_name: "",
      next_public_postiz_oauth_logo_url: "",
      postiz_generic_oauth: "",
      postiz_oauth_url: "",
      postiz_oauth_auth_url: "",
      postiz_oauth_token_url: "",
      postiz_oauth_userinfo_url: "",
      postiz_oauth_client_id: "",
      postiz_oauth_client_secret: "",
      postiz_oauth_scope: "",
      cloudflare_account_id: "",
      cloudflare_access_key: "",
      cloudflare_secret_access_key: "",
      cloudflare_bucketname: "",
      cloudflare_bucket_url: "",
      cloudflare_region: "",
      next_public_sentry_dsn: "",
      sentry_spotlight: "",
      loading: {
        getConfiguration: false,
        configureModule: false,
        restartPostiz: false,
      },
      error: {
        getConfiguration: "",
        configureModule: "",
        restartPostiz: "",
        host: "",
        temporal_host: "",
        lets_encrypt: "",
        http2https: "",
      },
    };
  },
  computed: {
    ...mapState(["instanceName", "core", "appName"]),
  },
  created() {
    this.getConfiguration();
  },
  beforeRouteEnter(to, from, next) {
    next((vm) => {
      vm.watchQueryData(vm);
      vm.urlCheckInterval = vm.initUrlBindingForApp(vm, vm.q.page);
    });
  },
  beforeRouteLeave(to, from, next) {
    clearInterval(this.urlCheckInterval);
    next();
  },
  methods: {
    async getConfiguration() {
      this.loading.getConfiguration = true;
      this.error.getConfiguration = "";
      const taskAction = "get-configuration";
      const eventId = this.getUuid();

      // register to task error
      this.core.$root.$once(
        `${taskAction}-aborted-${eventId}`,
        this.getConfigurationAborted,
      );

      // register to task completion
      this.core.$root.$once(
        `${taskAction}-completed-${eventId}`,
        this.getConfigurationCompleted,
      );

      const res = await to(
        this.createModuleTaskForApp(this.instanceName, {
          action: taskAction,
          extra: {
            title: this.$t("action." + taskAction),
            isNotificationHidden: true,
            eventId,
          },
        }),
      );
      const err = res[0];

      if (err) {
        console.error(`error creating task ${taskAction}`, err);
        this.error.getConfiguration = this.getErrorMessage(err);
        this.loading.getConfiguration = false;
        return;
      }
    },
    getConfigurationAborted(taskResult, taskContext) {
      console.error(`${taskContext.action} aborted`, taskResult);
      this.error.getConfiguration = this.$t("error.generic_error");
      this.loading.getConfiguration = false;
    },
    getConfigurationCompleted(taskContext, taskResult) {
      const config = taskResult.output;
      this.host = config.host;
      this.temporal_host = config.temporal_host;
      this.isLetsEncryptEnabled = config.lets_encrypt;
      this.isHttpToHttpsEnabled = config.http2https;

      // Postiz settings
      this.is_general = config.is_general || "true";
      this.next_public_upload_directory =
        config.next_public_upload_directory || "/uploads";
      this.github_client_secret = config.github_client_secret || "";
      this.beehiive_api_key = config.beehiive_api_key || "";
      this.youtube_client_id = config.youtube_client_id || "";
      this.dribbble_client_id = config.dribbble_client_id || "";
      this.facebook_app_secret = config.facebook_app_secret || "";
      this.storage_provider = config.storage_provider || "local";
      this.x_api_key = config.x_api_key || "";
      this.threads_app_secret = config.threads_app_secret || "";
      this.tiktok_client_secret = config.tiktok_client_secret || "";
      this.slack_secret = config.slack_secret || "";
      this.linkedin_client_secret = config.linkedin_client_secret || "";
      this.slack_signing_secret = config.slack_signing_secret || "";
      this.mastodon_url = config.mastodon_url || "https://mastodon.social";
      this.mastodon_client_secret = config.mastodon_client_secret || "";
      this.next_public_polotno = config.next_public_polotno || "";
      this.stripe_signing_key = config.stripe_signing_key || "";
      this.beehiive_publication_id = config.beehiive_publication_id || "";
      this.pinterest_client_id = config.pinterest_client_id || "";
      this.dribbble_client_secret = config.dribbble_client_secret || "";
      this.slack_id = config.slack_id || "";
      this.next_public_discord_support =
        config.next_public_discord_support || "";
      this.github_client_id = config.github_client_id || "";
      this.discord_client_secret = config.discord_client_secret || "";
      this.openai_api_key = config.openai_api_key || "";
      this.disable_registration = config.disable_registration || "false";
      this.run_cron = config.run_cron || "true";
      this.reddit_client_secret = config.reddit_client_secret || "";
      this.fee_amount = config.fee_amount || "0.05";
      this.nx_add_plugins = config.nx_add_plugins || "false";
      this.linkedin_client_id = config.linkedin_client_id || "";
      this.threads_app_id = config.threads_app_id || "";
      this.facebook_app_id = config.facebook_app_id || "";
      this.youtube_client_secret = config.youtube_client_secret || "";
      this.tiktok_client_id = config.tiktok_client_id || "";
      this.pinterest_client_secret = config.pinterest_client_secret || "";
      this.mastodon_client_id = config.mastodon_client_id || "";
      this.x_api_secret = config.x_api_secret || "";
      this.reddit_client_id = config.reddit_client_id || "";
      this.discord_client_id = config.discord_client_id || "";
      this.discord_bot_token_id = config.discord_bot_token_id || "";
      this.api_limit = config.api_limit || "30";
      this.stripe_publishable_key = config.stripe_publishable_key || "";
      this.stripe_secret_key = config.stripe_secret_key || "";
      this.stripe_signing_key_connect = config.stripe_signing_key_connect || "";
      this.email_provider = config.email_provider || "nodemailer";
      this.email_host = config.email_host || "";
      this.email_port = config.email_port || "465";
      this.email_user = config.email_user || "";
      this.email_pass = config.email_pass || "";
      this.email_secure = config.email_secure || "true";
      this.dub_token = config.dub_token || "";
      this.dub_api_endpoint = config.dub_api_endpoint || "";
      this.dub_short_link_domain = config.dub_short_link_domain || "";
      this.short_io_secret_key = config.short_io_secret_key || "";
      this.kutt_api_key = config.kutt_api_key || "";
      this.kutt_api_endpoint = config.kutt_api_endpoint || "";
      this.kutt_short_link_domain = config.kutt_short_link_domain || "";
      this.link_drip_api_key = config.link_drip_api_key || "";
      this.link_drip_api_endpoint = config.link_drip_api_endpoint || "";
      this.link_drip_short_link_domain = config.link_drip_short_link_domain || "";
      this.next_public_postiz_oauth_display_name = config.next_public_postiz_oauth_display_name || "";
      this.next_public_postiz_oauth_logo_url = config.next_public_postiz_oauth_logo_url || "";
      this.postiz_generic_oauth = config.postiz_generic_oauth || "";
      this.postiz_oauth_url = config.postiz_oauth_url || "";
      this.postiz_oauth_auth_url = config.postiz_oauth_auth_url || "";
      this.postiz_oauth_token_url = config.postiz_oauth_token_url || "";
      this.postiz_oauth_userinfo_url = config.postiz_oauth_userinfo_url || "";
      this.postiz_oauth_client_id = config.postiz_oauth_client_id || "";
      this.postiz_oauth_client_secret = config.postiz_oauth_client_secret || "";
      this.postiz_oauth_scope = config.postiz_oauth_scope || "";
      this.cloudflare_account_id = config.cloudflare_account_id || "";
      this.cloudflare_access_key = config.cloudflare_access_key || "";
      this.cloudflare_secret_access_key = config.cloudflare_secret_access_key || "";
      this.cloudflare_bucketname = config.cloudflare_bucketname || "";
      this.cloudflare_bucket_url = config.cloudflare_bucket_url || "";
      this.cloudflare_region = config.cloudflare_region || "";
      this.next_public_sentry_dsn = config.next_public_sentry_dsn || "";
      this.sentry_spotlight = config.sentry_spotlight || "";

      this.loading.getConfiguration = false;
      this.focusElement("host");
    },
    validateConfigureModule() {
      this.clearErrors(this);

      let isValidationOk = true;
      if (!this.host) {
        this.error.host = "common.required";

        if (isValidationOk) {
          this.focusElement("host");
        }
        isValidationOk = false;
      }
      if (!this.temporal_host) {
        this.error.temporal_host = "common.required";

        if (isValidationOk) {
          this.focusElement("temporal_host");
        }
        isValidationOk = false;
      }
      return isValidationOk;
    },
    configureModuleValidationFailed(validationErrors) {
      this.loading.configureModule = false;
      let focusAlreadySet = false;

      for (const validationError of validationErrors) {
        const param = validationError.parameter;
        // set i18n error message
        this.error[param] = this.$t("settings." + validationError.error);

        if (!focusAlreadySet) {
          this.focusElement(param);
          focusAlreadySet = true;
        }
      }
    },
    async configureModule() {
      this.error.test_imap = false;
      this.error.test_smtp = false;
      const isValidationOk = this.validateConfigureModule();
      if (!isValidationOk) {
        return;
      }

      this.loading.configureModule = true;
      const taskAction = "configure-module";
      const eventId = this.getUuid();

      // register to task error
      this.core.$root.$once(
        `${taskAction}-aborted-${eventId}`,
        this.configureModuleAborted,
      );

      // register to task validation
      this.core.$root.$once(
        `${taskAction}-validation-failed-${eventId}`,
        this.configureModuleValidationFailed,
      );

      // register to task completion
      this.core.$root.$once(
        `${taskAction}-completed-${eventId}`,
        this.configureModuleCompleted,
      );
      const res = await to(
        this.createModuleTaskForApp(this.instanceName, {
          action: taskAction,
          data: {
            host: this.host,
            temporal_host: this.temporal_host,
            lets_encrypt: this.isLetsEncryptEnabled,
            http2https: this.isHttpToHttpsEnabled,
            // Postiz settings
            is_general: this.is_general,
            next_public_upload_directory: this.next_public_upload_directory,
            github_client_secret: this.github_client_secret,
            beehiive_api_key: this.beehiive_api_key,
            youtube_client_id: this.youtube_client_id,
            dribbble_client_id: this.dribbble_client_id,
            facebook_app_secret: this.facebook_app_secret,
            storage_provider: this.storage_provider,
            x_api_key: this.x_api_key,
            threads_app_secret: this.threads_app_secret,
            tiktok_client_secret: this.tiktok_client_secret,
            slack_secret: this.slack_secret,
            linkedin_client_secret: this.linkedin_client_secret,
            slack_signing_secret: this.slack_signing_secret,
            mastodon_url: this.mastodon_url,
            mastodon_client_secret: this.mastodon_client_secret,
            next_public_polotno: this.next_public_polotno,
            stripe_signing_key: this.stripe_signing_key,
            beehiive_publication_id: this.beehiive_publication_id,
            pinterest_client_id: this.pinterest_client_id,
            dribbble_client_secret: this.dribbble_client_secret,
            slack_id: this.slack_id,
            next_public_discord_support: this.next_public_discord_support,
            github_client_id: this.github_client_id,
            discord_client_secret: this.discord_client_secret,
            openai_api_key: this.openai_api_key,
            disable_registration: this.disable_registration,
            run_cron: this.run_cron,
            reddit_client_secret: this.reddit_client_secret,
            fee_amount: this.fee_amount,
            nx_add_plugins: this.nx_add_plugins,
            linkedin_client_id: this.linkedin_client_id,
            threads_app_id: this.threads_app_id,
            facebook_app_id: this.facebook_app_id,
            youtube_client_secret: this.youtube_client_secret,
            tiktok_client_id: this.tiktok_client_id,
            pinterest_client_secret: this.pinterest_client_secret,
            mastodon_client_id: this.mastodon_client_id,
            x_api_secret: this.x_api_secret,
            reddit_client_id: this.reddit_client_id,
            discord_client_id: this.discord_client_id,
            discord_bot_token_id: this.discord_bot_token_id,
            api_limit: this.api_limit,
            stripe_publishable_key: this.stripe_publishable_key,
            stripe_secret_key: this.stripe_secret_key,
            stripe_signing_key_connect: this.stripe_signing_key_connect,
            email_provider: this.email_provider,
            email_host: this.email_host,
            email_port: this.email_port,
            email_user: this.email_user,
            email_pass: this.email_pass,
            email_secure: this.email_secure,
            dub_token: this.dub_token,
            dub_api_endpoint: this.dub_api_endpoint,
            dub_short_link_domain: this.dub_short_link_domain,
            short_io_secret_key: this.short_io_secret_key,
            kutt_api_key: this.kutt_api_key,
            kutt_api_endpoint: this.kutt_api_endpoint,
            kutt_short_link_domain: this.kutt_short_link_domain,
            link_drip_api_key: this.link_drip_api_key,
            link_drip_api_endpoint: this.link_drip_api_endpoint,
            link_drip_short_link_domain: this.link_drip_short_link_domain,
            next_public_postiz_oauth_display_name: this.next_public_postiz_oauth_display_name,
            next_public_postiz_oauth_logo_url: this.next_public_postiz_oauth_logo_url,
            postiz_generic_oauth: this.postiz_generic_oauth,
            postiz_oauth_url: this.postiz_oauth_url,
            postiz_oauth_auth_url: this.postiz_oauth_auth_url,
            postiz_oauth_token_url: this.postiz_oauth_token_url,
            postiz_oauth_userinfo_url: this.postiz_oauth_userinfo_url,
            postiz_oauth_client_id: this.postiz_oauth_client_id,
            postiz_oauth_client_secret: this.postiz_oauth_client_secret,
            postiz_oauth_scope: this.postiz_oauth_scope,
            cloudflare_account_id: this.cloudflare_account_id,
            cloudflare_access_key: this.cloudflare_access_key,
            cloudflare_secret_access_key: this.cloudflare_secret_access_key,
            cloudflare_bucketname: this.cloudflare_bucketname,
            cloudflare_bucket_url: this.cloudflare_bucket_url,
            cloudflare_region: this.cloudflare_region,
            next_public_sentry_dsn: this.next_public_sentry_dsn,
            sentry_spotlight: this.sentry_spotlight,
          },
          extra: {
            title: this.$t("settings.instance_configuration", {
              instance: this.instanceName,
            }),
            description: this.$t("settings.configuring"),
            eventId,
          },
        }),
      );
      const err = res[0];

      if (err) {
        console.error(`error creating task ${taskAction}`, err);
        this.error.configureModule = this.getErrorMessage(err);
        this.loading.configureModule = false;
        return;
      }
    },
    configureModuleAborted(taskResult, taskContext) {
      console.error(`${taskContext.action} aborted`, taskResult);
      this.error.configureModule = this.$t("error.generic_error");
      this.loading.configureModule = false;
    },
    configureModuleCompleted() {
      this.loading.configureModule = false;

      // reload configuration
      this.getConfiguration();
    },
    async restartPostiz() {
      this.loading.restartPostiz = true;
      this.error.restartPostiz = "";
      const taskAction = "restart-postiz";
      const eventId = this.getUuid();

      // register to task error
      this.core.$root.$once(
        `${taskAction}-aborted-${eventId}`,
        this.restartPostizAborted
      );

      // register to task completion
      this.core.$root.$once(
        `${taskAction}-completed-${eventId}`,
        this.restartPostizCompleted
      );

      const res = await to(
        this.createModuleTaskForApp(this.instanceName, {
          action: taskAction,
          extra: {
            title: this.$t("action." + taskAction),
            description: this.$t("settings.restarting_postiz"),
            eventId,
          },
        })
      );
      const err = res[0];

      if (err) {
        console.error(`error creating task ${taskAction}`, err);
        this.error.restartPostiz = this.getErrorMessage(err);
        this.loading.restartPostiz = false;
        return;
      }
    },
    restartPostizAborted(taskResult, taskContext) {
      console.error(`${taskContext.action} aborted`, taskResult);
      this.error.restartPostiz = this.$t("error.generic_error");
      this.loading.restartPostiz = false;
    },
    restartPostizCompleted() {
      this.loading.restartPostiz = false;
    },
  },
};
</script>

<style scoped lang="scss">
@import "../styles/carbon-utils";
.mg-bottom {
  margin-bottom: $spacing-06;
}

.mg-left {
  margin-left: $spacing-05;
}

.maxwidth {
  max-width: 38rem;
}

.restart-description {
  margin-top: $spacing-05;
  font-size: 0.875rem;
  color: $text-03;
}
</style>
