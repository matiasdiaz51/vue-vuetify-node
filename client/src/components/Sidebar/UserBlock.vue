<template>
	<v-list-tile class="sidebar-profile">
		<v-list-tile-avatar>
			<img src="/static/avatars/avatar.png" alt="avatar" height="40" width="40" class="img-responsive" />
		</v-list-tile-avatar>
		<v-list-tile-content class="mx-2">
			<v-list-tile-title class="admin-name">
				<span v-show="getUser !== null">{{getUser.first_name + ' ' + getUser.last_name}}</span>
			</v-list-tile-title>
		</v-list-tile-content>
		<v-menu bottom offset-y left content-class="userblock-dropdown" nudge-top="-10" nudge-right="0" transition="slide-y-transition">
			<v-btn dark icon slot="activator" class="ma-0">
				<v-icon>more_vert</v-icon>
			</v-btn>
			<div class="dropdown-content">
				<div class="dropdown-top white--text primary">
					<span v-show="getUser !== null" class="white--text fs-14 fw-bold d-block">{{getUser.first_name + ' ' + getUser.last_name}}</span>
					<span v-show="getUser !== null" class="d-block fs-12 fw-light">{{getUser.email}}</span>
				</div>
				<v-list class="dropdown-list">
					<template v-for="userLink in userLinks" v-if="userLink.id !== 4">
						<v-list-tile :to="getMenuLink(userLink.path)" :key="userLink.id">
							<i :class="userLink.icon"></i>
							<span>{{$t(userLink.title)}}</span>
						</v-list-tile>
					</template>
					<template v-else>
						<v-list-tile @click="logoutUser" :key="userLink.id">
							<i :class="userLink.icon"></i>
							<span>{{$t(userLink.title)}}</span>
						</v-list-tile>
					</template>
				</v-list>
			</div>
		</v-menu>
	</v-list-tile>
</template>

<script>
import { getCurrentAppLayout } from "Helpers/helpers";
import { mapGetters } from "vuex";

export default {
  data() {
    return {
      userLinks: [
        {
          id: 4,
          title: "message.logOut",
          icon: "ti-power-off mr-3 error--text"
        }
      ]
    };
  },
  methods: {
    logoutUser() {
      this.$store.dispatch("logout", this.$router);
    },
    getMenuLink(path) {
      return "/" + getCurrentAppLayout(this.$router) + path;
    }
  },
  computed: {
    ...mapGetters(["getUser", "selectedUser"])
  }
};
</script>
<style lang="scss" scoped>
.admin-name {
	text-transform: capitalize;
}
</style>