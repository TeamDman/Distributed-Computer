<template>
	<b-container class="ma-2">
		<div class="d-flex justify-content-center">
			<b-button @click="onClick">run</b-button>
		</div>
		<div class="d-flex flex-column text-whtie flex-wrap">
			<div v-for="(v, i) of results" :key="i" class="p-2 bd-highlight" :class="{'bg-error':!v.passed, 'bg-primary':v.passed}">
				{{ v.passed ? "Passed!" : "Failure!" }}
			</div>
		</div>
	</b-container>
</template>

<script>
import HelloWorld from "@/components/HelloWorld.vue";

export default {
	name: "home",
	components: {},
	data() {
		return {
			slices:10,
			answer: "correct",
			results: [],
		};
	},
	methods: {
		async onClick() {
			const { compute } = dcp;
			const job = compute.for(0, this.slices, (i)=> {
				progress(1);
				return "yeet";
			});
			job.on("result", ev => {
				console.log(`Result ${ev.sliceNumber} ${ev.result}`);
				this.results.push({
					passed:ev.result == "yeet",
					ev,
				});
			});
			console.log(`Deploying job with ${this.slices} slices.`);
			const results = await job.exec(0.00001);
			console.log("Finished");
			console.log(results);
		},
	},
};
</script>
