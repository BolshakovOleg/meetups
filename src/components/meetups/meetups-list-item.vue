<template>
  <router-link
    class="meetups-list__item"
    :to="{ name: 'meetup', params: { id: meetup.id } }"
  >
    <div class="meetups-list__col">
      <div
        class="meetups-list__cover"
        :style="meetup.cover ? `--bg-url: url('${meetup.cover}')` : ''"
      >
        <h5>{{ meetup.title }}</h5>
      </div>
    </div>
    <div class="meetups-list__col">
      <div class="meetups-list__description">
        <span
          v-if="meetup.attending"
          class="meetups-list__badge meetups-list__badge_success"
          >Участвую</span
        >
        <span v-if="meetup.organizing" class="meetups-list__badge"
          >Организую</span
        >
        <ul class="info-list">
          <li>
            <app-icon icon="user" class="info-list__icon" />
            {{ meetup.organizer }}
          </li>
          <li>
            <app-icon icon="map" class="info-list__icon" />
            {{ meetup.place }}
          </li>
          <li>
            <app-icon icon="cal-lg" class="info-list__icon" />
            <time :datetime="meetup.ISODate">{{ meetup.localDate }}</time>
          </li>
        </ul>
      </div>
    </div>
  </router-link>
</template>

<script>
import AppIcon from "@/components/base/app-icon";

export default {
  name: "meetups-list-item",
  components: { AppIcon },
  props: {
    meetup: {
      required: true,
      type: Object
    }
  }
};
</script>

<style scoped></style>
