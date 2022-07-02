<template>
	<swiper v-bind="swiperSettings" class="h-full" @swiper="onSwiper" @slideChange="onSlideChange">
		<swiper-slide v-for="(video, i) in videos" :key="i">
			<div class="flex w-full h-full bg-black relative">
				<video class="w-full object-contain" loop playsinline webkit-playsinline :poster="video.thumbSrc" :ref="'video' + i" @click="toggleActive">
					<source :src="video.src" type="video/mp4"> 
					{{videoUnsupported}}
				</video>
				<ion-icon v-if="isActivePaused" :icon="icons.play" class="text-[#eee] absolute top-1/2 left-1/2 translate-x-[-50%] translate-y-[-50%] opacity-50 text-[82px]" @click="toggleActive"></ion-icon>

				<ion-icon :icon="video.muted ? icons.volumeMute : icons.volumeHigh" class="absolute top-[10%] left-4 bg-white rounded-full text-[34px] p-2 drop-shadow swiper-no-swiping" @click="toggleMuteActive(video)"></ion-icon>
				<div class="flex flex-col items-center text-white absolute bottom-[10%] right-4 swiper-no-swiping">
					<div class="flex flex-col items-center drop-shadow" @click="like(video)">
						<ion-icon :icon="icons.heart" class="text-[34px] p-2" :class="{liked: video.liked}"></ion-icon>
						<p class="m-0">{{video.likes = video.likes == null ? 0 : video.likes}}</p>
					</div>
					<div class="flex flex-col items-center drop-shadow" @click="comment(video)">
						<ion-icon :icon="icons.chatbubbleEll" class="text-[34px] p-2"></ion-icon>
						<p class="m-0">{{video.comments = video.comments == null ? 0 : video.comments}}</p>
					</div>
					<div class="flex flex-col items-center drop-shadow" @click="save(video)">
						<ion-icon :icon="icons.bookmark" class="text-[34px] p-2" :class="{shared: video.shared}"></ion-icon>
						<p class="m-0">{{saveTxt}}</p>
					</div>
					<div class="flex flex-col items-center drop-shadow" @click="share(video)">
						<ion-icon :icon="icons.arrowRedo" class="text-[34px] p-2"></ion-icon>
						<p class="m-0">{{shareTxt}}</p>
					</div>
				</div>
			</div>
		</swiper-slide>
	</swiper>
</template>

<script>
import { Mousewheel } from "swiper";

export default {
	props: {
		videos: {
			type: Array
		},

		videoUnsupported: {
			type: String,
			default: "Your browser does not support the video tag."
		},

		saveTxt: {
			type: String,
			default: "Save"
		},

		shareTxt: {
			type: String,
			default: "Share"
		}
	},

	methods: {
		onSwiper(swiper) {
			this.swiper = swiper;
			setTimeout(() => this.onSlideChange(), 500);
		},

		onSlideChange() {
			if (this.activeVideo)
			{
				this.activeVideo.load();
				this.activeVideo.pause();
			}
			
			if (this.activeVideo = this.$refs["video" + this.swiper.activeIndex])
			{
				this.activeVideo = this.activeVideo[0];
				this.playActive();
			}
		},

		toggleActive() {
			this.activeVideo.paused ? this.playActive() : this.pauseActive();
		},

		playActive() {
			if (this.activeVideo.paused)
			{
				this.activeVideo.play();
				this.isActivePaused = false;
			}
		},

		pauseActive() {
			if (!this.activeVideo.paused)
			{
				this.activeVideo.pause();
				this.isActivePaused = true;
			}
		},

		toggleMuteActive(video) {
			this.activeVideo.muted = video.muted ^= true;
		},

		like(video) {
			video.likes += !(video.liked ^= true) ? -1 : 1;
		},

		comment(video) {
			console.log("commenting", video);
		},

		save(video) {
			video.shared ^= true;
		},

		share(video) {
			navigator.share({
				title: video.name == null ? "Video" : video.name,
				url: video.src
			});
		}
	},

	data() {
		return {
			swiper: null,
			activeVideo: null,
			isActivePaused: false,

			swiperSettings: {
				slidesPerView: 1,
				spaceBetween: 0,
				direction: "vertical",
				mousewheel: true,
				modules: [Mousewheel]
			}
		}
	}
}
</script>

<style lang="scss" scoped>
.liked {
	color: red;
}

.shared {
	color: orange;
}
</style>