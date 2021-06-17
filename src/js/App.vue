<template>
	<div id="appex">
		<div class="x-flx m__flxColumn xu__flx-center xm__mX xm__p">
			<div class="x-txt">
				<h1>Numeros aleatorios</h1>
			</div>
			<div class="x-txt xm__txtAlign-center">
				<p>
					<b class="xm__txtColor-primary7">Se repite en 2 tablas</b>
					|
					<b class="xm__txtColor-danger7">Se repite en 3 tablas</b>
				</p>
			</div>
			<div class="x-flx m__flxRow-wrap xu__flx-center">
				<button
					v-if="oldRows.length && oldRows[0] !== rows[0]"
					class="x-link"
					@click="turnBack()"
				>
					<i class="fas fa-arrow-left x-icon"></i>
					<span>Numeros previos</span>
				</button>
				<button class="x-btn" @click="populate()">Generar numeros</button>
			</div>
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
							<td v-if="!rows[0]"></td>
							<td
								v-else
								:class="[
									{ is__firstMatch: rows[1].includes(value) },
									{ is__secondMatch: rows[2].includes(value) },
								]"
								:title="
									(rows[1].includes(value)
										? ` Tabla 2, fila ${rows[1].indexOf(value) + 1};`
										: '') +
										(rows[2].includes(value)
											? ` Tabla 3, fila ${rows[2].indexOf(value) + 1};`
											: '')
								"
							>
								{{ rows[0] ? addZeros(value) : "" }}
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
									{ is__secondMatch: rows[2].includes(value) },
								]"
								:title="
									(rows[0].includes(value)
										? ` Tabla 1, fila ${rows[0].indexOf(value) + 1};`
										: '') +
										(rows[2].includes(value)
											? ` Tabla 3, fila ${rows[2].indexOf(value) + 1};`
											: '')
								"
							>
								{{ addZeros(value) }}
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
									{ is__secondMatch: rows[0].includes(value) },
								]"
								:title="
									(rows[1].includes(value)
										? ` Tabla 2, fila ${rows[1].indexOf(value) + 1};`
										: '') +
										(rows[0].includes(value)
											? ` Tabla 1, fila ${rows[0].indexOf(value) + 1};`
											: '')
								"
							>
								{{ addZeros(value) }}
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
				oldRows: [],
			};
		},
		methods: {
			addZeros(int, amount = 4) {
				int = int.toString();
				if (int.length !== amount) {
					while (int.length < amount) {
						int = `0${int}`;
					}
				}
				return int;
			},
			getRandomNumber(existingNumbers = [], minimum = 0, maximum = 9999) {
				const random = Math.floor(Math.random() * (maximum - minimum + 1)) + minimum;
				if (existingNumbers.includes(random)) return this.getRandomNumber(existingNumbers);
				return random;
			},
			populate() {
				let newRows = [[], [], []];
				this.oldRows = [...this.rows];
				for (let t = 0; t < newRows.length; t++) {
					for (let n = 0; n < 100; n++) {
						newRows[t].push(this.getRandomNumber(newRows[t]));
					}
				}
				this.rows = newRows;
			},
			turnBack() {
				this.rows = [...this.oldRows];
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
