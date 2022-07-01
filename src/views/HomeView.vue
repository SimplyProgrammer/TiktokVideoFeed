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
				const reply = (await Axios.get("https://gamechangers.wezeo.dev/cms/api/v1/lesson/feed/recommended/")).data.data; //Certified JS moment...
				for (let i = 0; i < reply.length; i++) {
					const element = reply[i];
					if (element.content_shot)
						this.videos.push({name: element.name, src: element.content_shot, thumbSrc: element.content_thumbnail, likes: element.likes});
				}
			}
			catch (e) {
				this.toast("BE mate spomaleny ja za to nemozem...", "danger");
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