<template>
<div>
  <div v-if="user.id" class="user-profile panel panel-default">
    <user-card-content
      :user="user"
      :switcher="true"
      :selected="timeline.viewing"
    />
    <tab-switcher :renderOnlyFocused="true">
      <Timeline
        :label="$t('user_card.statuses')"
        :disabled="!user.statuses_count"
        :embedded="true"
        :title="$t('user_profile.timeline_title')"
        :timeline="timeline"
        :timeline-name="'user'"
        :user-id="fetchBy"
      />
      <div :label="$t('user_card.followees')" v-if="followsTabVisible" :disabled="!user.friends_count">
        <FollowList v-if="user.friends_count > 0" :userId="userId" :showFollowers="false" />
        <div class="userlist-placeholder" v-else>
          <i class="icon-spin3 animate-spin"></i>
        </div>
      </div>
      <div :label="$t('user_card.followers')" v-if="followersTabVisible" :disabled="!user.followers_count">
        <FollowList v-if="user.followers_count > 0" :userId="userId" :showFollowers="true" />
        <div class="userlist-placeholder" v-else>
          <i class="icon-spin3 animate-spin"></i>
        </div>
      </div>
      <Timeline
        :label="$t('user_card.media')"
        :disabled="!media.visibleStatuses.length"
        :embedded="true" :title="$t('user_card.media')"
        timeline-name="media"
        :timeline="media"
        :user-id="fetchBy"
      />
      <Timeline
        v-if="isUs"
        :label="$t('user_card.favorites')"
        :disabled="!favorites.visibleStatuses.length"
        :embedded="true"
        :title="$t('user_card.favorites')"
        timeline-name="favorites"
        :timeline="favorites"
      />
    </tab-switcher>
  </div>
  <div v-else class="panel user-profile-placeholder">
    <div class="panel-heading">
      <div class="title">
        {{ $t('settings.profile_tab') }}
      </div>
    </div>
    <div class="panel-body">
      <i class="icon-spin3 animate-spin"></i>
    </div>
  </div>
</div>
</template>

<script src="./user_profile.js"></script>

<style lang="scss">

.user-profile {
  flex: 2;
  flex-basis: 500px;

  .profile-panel-background .panel-heading {
    background: transparent;
    flex-direction: column;
    align-items: stretch;
  }
  .userlist-placeholder {
    display: flex;
    justify-content: center;
    align-items: middle;
    padding: 2em;
  }

  .timeline-heading {
    display: flex;
    justify-content: center;

    .loadmore-button, .alert {
      flex: 1;
    }

    .loadmore-button {
      height: 28px;
      margin: 10px .6em;
    }

    .title, .loadmore-text {
      display: none
    }
  }
}
.user-profile-placeholder {
  .panel-body {
    display: flex;
    justify-content: center;
    align-items: middle;
    padding: 7em;
  }
}
</style>
