<!-- Side Structure -->
<template>
	<div class="sidebar" :class="sidebarSelectedFilter.class">
		<vue-perfect-scrollbar class="scroll-area" :settings="settings">
			<v-toolbar flat class="transparent scroll-area navigation">
				<v-list>
					<app-logo></app-logo>
					<user-block></user-block>
					<template v-for="(category, key) in menus">
						<div :key="key">
							<div class="sidebar-title px-3">
								<span>{{$t(key)}}</span>
							</div>
							<template v-for="item in category">
								<template v-if="item.items!= null">
									<v-list-group :key="item.title" prepend-icon="ti-arrow-right" append-icon="" no-action v-model="item.active">
										<v-list-tile slot="activator">
											<v-list-tile-content>
												<v-list-tile-title>
													<i :class="item.icon"></i>
													<span>{{ textTruncate($t(item.title)) }}</span>
												</v-list-tile-title>
											</v-list-tile-content>
										</v-list-tile>
										<v-list-tile v-for="subItem in item.items" v-bind:key="subItem.title" v-if="subItem !== null" :to="!subItem.exact ? `/${getCurrentAppLayoutHandler() + subItem.path}` : subItem.path">
											<v-list-tile-content>
												<i :class="subItem.icon"></i>
												<v-list-tile-title>{{ textTruncate($t(subItem.title)) }}</v-list-tile-title>
											</v-list-tile-content>
										</v-list-tile>
									</v-list-group>
								</template>
								<template v-else>
									<v-list-tile :to="!item.exact ? `/${getCurrentAppLayoutHandler() + item.path}` : item.path" :key="item.path">
										<v-list-tile-action>
											<i :class="item.icon"></i>
										</v-list-tile-action>
										<v-list-tile-content>
											<v-list-tile-title>
												<i class="zmdi" :class="item.action"></i>
												<span>{{ textTruncate($t(item.title)) }}</span>
											</v-list-tile-title>
										</v-list-tile-content>
									</v-list-tile>
								</template>
							</template>
						</div>
					</template>
				</v-list>
			</v-toolbar>
		</vue-perfect-scrollbar>
	</div>
</template>

<script>
import UserBlock from "./UserBlock";
import { textTruncate, getCurrentAppLayout } from "Helpers/helpers";
import { mapGetters } from "vuex";
import AppLogo from "Components/AppLogo/AppLogo";

export default {
  data() {
    return {
      settings: {
        maxScrollbarLength: 160
      }
    };
  },
  components: {
    UserBlock,
    AppLogo
  },
  computed: {
    ...mapGetters(["sidebarSelectedFilter", "menus"])
  },
  mounted() {
    this.$store.dispatch("resetMenu");
    // this.$store.dispatch("setActiveMenuGroup", this.$router);
  },
  methods: {
    textTruncate(text) {
      return textTruncate(text, 18);
    },
    getCurrentAppLayoutHandler() {
      return getCurrentAppLayout(this.$router);
    }
  }
};
</script>
