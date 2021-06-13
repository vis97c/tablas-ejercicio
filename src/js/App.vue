<template>
	<div id="appex">
		<div class="x-flx m__flxColumn xu__flx-center xm__mX xm__p">
			<div class="x-txt">
				<h1>tablas</h1>
			</div>
			<button class="x-btn" @click="populate()">Generar numeros</button>
			<div class="x-flx m__flxRow-wrap xu__flx-center-start">
				<table class="x-table">
					<thead>
						<tr>
							<th class="xm__txtAlign-center" colspan="50">Tabla 1</th>
						</tr>
					</thead>
					<tbody>
						<tr v-for="(value, index) in rows[0] || 100" :key="index">
							<th>Fila: {{ index + 1 }}</th>
							<td
								:class="
									rows[0]
										? [
												{ is__firstMatch: rows[1].includes(value) },
												{ is__firstMatch: rows[2].includes(value) },
										  ]
										: false
								"
							>
								{{ rows[0] ? value : "" }}
							</td>
						</tr>
					</tbody>
				</table>
				<table v-if="rows[1]" class="x-table">
					<thead>
						<tr>
							<th class="xm__txtAlign-center" colspan="50">Tabla 2</th>
						</tr>
					</thead>
					<tbody>
						<tr v-for="(value, index) in rows[1]" :key="index">
							<td
								:class="[
									{ is__firstMatch: rows[0].includes(value) },
									{ is__firstMatch: rows[2].includes(value) },
								]"
							>
								{{ value }}
							</td>
						</tr>
					</tbody>
				</table>
				<table v-if="rows[2]" class="x-table">
					<thead>
						<tr>
							<th class="xm__txtAlign-center" colspan="50">Tabla 3</th>
						</tr>
					</thead>
					<tbody>
						<tr v-for="(value, index) in rows[2]" :key="index">
							<td
								:class="[
									{ is__firstMatch: rows[1].includes(value) },
									{ is__firstMatch: rows[0].includes(value) },
								]"
							>
								{{ value }}
							</td>
						</tr>
					</tbody>
				</table>
			</div>
		</div>
	</div>
</template>

<script>
	export default {
		name: "App",
		data() {
			return {
				rows: [],
			};
		},
		methods: {
			getRandomNumber(existingNumbers = [], minimum = 1000, maximum = 9999) {
				const random = Math.floor(Math.random() * (maximum - minimum + 1)) + minimum;
				if (existingNumbers.includes(random)) return this.getRandomNumber(existingNumbers);
				return random;
			},
			populate() {
				let newRows = [[], [], []];
				for (let t = 0; t < newRows.length; t++) {
					for (let n = 0; n < 100; n++) {
						newRows[t].push(this.getRandomNumber(newRows[t]));
					}
				}
				this.rows = newRows;
			},
		},
	};
</script>

<style lang="scss">
	@import "@xamu-co/styles/src/utils/module.scss";
	.is__firstMatch:not(.is__secondMatch),
	.is__secondMatch:not(.is__firstMatch) {
		// 1 match only: yellow
		background: color(primary, 0.3);
	}
	.is__firstMatch.is__secondMatch {
		// 2 matches: red
		background: color(danger, 0.3);
	}
</style>
