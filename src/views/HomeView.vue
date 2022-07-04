<template>
	 <ion-page>
		<ion-content>
			<Z-Infinity-Video-Feed :videos="videos" class="md:w-[50%]" @reachEnd="generateRandomMock()"></Z-Infinity-Video-Feed>
		</ion-content> 
	</ion-page>
</template>

<script>
import ZInfinityVideoFeed from "@/components/ZInfinityVideoFeed.vue";
import Axios from "axios";

export default {
	components: {ZInfinityVideoFeed},

	async mounted() {
		await this.generateRandomMock();
	},

	methods: {
		async generateRandomMock() { //Load mocks
			try {		
				const { data: { data: reply } } = await Axios.get("https://gamechangers.wezeo.dev/cms/api/v1/lesson/feed/recommended/");
				this.videos.push(...reply.filter(element => element.content_shot));
			}
			catch (e) {
				this.toast(e, "danger");
			}
		}
	},

	data() {
		return {
			videos: [],
		}
	}
}
</script>

<style lang="scss" scoped>
ion-content{
    --ion-background-color: rgb(68, 68, 68);
}
</style>