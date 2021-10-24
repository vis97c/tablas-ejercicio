<template>
	<div id="appex">
		<div v-if="rows.length" class="u-floating x-flx m__flxRow xu__flx-center xm__gaping-5">
			<div
				v-if="!singleMatches.length && !multipleMatches.length"
				class="x-box xm__txtAlign-center xm__txtColor-secondary"
			>
				no se detectaron coincidencias
			</div>
			<a
				v-if="singleMatches.length"
				class="x-btn m__round u__tm-primary-light"
				href="#two"
				:data-tooltip="
					`${singleMatches.length} coincidencia${singleMatches.length > 1 ? 's' : ''}`
				"
				data-tooltip-text
			>
				{{ singleMatches.length }}
			</a>
			<a
				v-if="multipleMatches.length"
				class="x-btn m__round  u__tm-success-light"
				href="#three"
				:data-tooltip="
					`${multipleMatches.length} coincidencia${multipleMatches.length > 1 ? 's' : ''}`
				"
				data-tooltip-text
			>
				{{ multipleMatches.length }}
			</a>
		</div>
		<div class="x-flx m__flxColumn xu__flx-center xm__mX xm__p">
			<div class="x-txt xm__txtAlign-center">
				<h1>Numeros aleatorios</h1>
			</div>
			<div class="x-txt xm__txtAlign-center xm__gaping-5">
				<p>
					<b class="xm__txtColor-primary">Se repite en 2 tablas</b>
					|
					<b class="xm__txtColor-danger">Se repite en 3 tablas</b>
				</p>
				<p v-if="rows.length">
					Mostrando la tabla
					<b>#{{ activeRowsIndex + 1 }}</b>
					de {{ rows.length }} tablas generadas
				</p>
			</div>
			<div class="x-flx m__flxRow-wrap xu__flx-center">
				<button
					v-if="rows.length > 1 && activeRowsIndex > 0"
					class="x-link"
					data-tooltip="Numeros previos"
					@click="goBack()"
				>
					<i class="fas fa-arrow-left x-icon"></i>
					<span class="xm__hidden:md-inv">Numeros previos</span>
				</button>
				<button class="x-btn" @click="populate()">Generar numeros</button>
				<button
					v-if="rows.length > 1 && activeRowsIndex < rows.length - 1"
					class="x-link"
					data-tooltip="Numeros siguientes"
					@click="goForward()"
				>
					<span class="xm__hidden:md-inv">Numeros siguientes</span>
					<i class="fas fa-arrow-right x-icon"></i>
				</button>
			</div>
			<div class="x-flx m__flxRow-wrap xu__flx-center-start xm__gaping-5">
				<table class="x-table">
					<thead>
						<tr>
							<th class="xm__txtAlign-center" colspan="50">Tabla 1</th>
						</tr>
					</thead>
					<tbody>
						<tr v-for="(value, index) in getRows[0] || 100" :key="index">
							<th class="xm__hidden:md-inv">Fila: {{ index + 1 }}</th>
							<td v-if="!rows[0]"></td>
							<td
								v-else
								:class="[
									'xm__txtAlign-center',
									{ is__firstMatch: getRows[1].includes(value) },
									{ is__secondMatch: getRows[2].includes(value) },
								]"
								:title="
									(getRows[1].includes(value)
										? ` Tabla 2, fila ${getRows[1].indexOf(value) + 1};`
										: '') +
										(getRows[2].includes(value)
											? ` Tabla 3, fila ${getRows[2].indexOf(value) + 1};`
											: '')
								"
							>
								<code>{{ getRows[0] ? addZeros(value) : "" }}</code>
							</td>
						</tr>
					</tbody>
				</table>
				<table v-if="getRows[1]" class="x-table">
					<thead>
						<tr>
							<th class="xm__txtAlign-center" colspan="50">Tabla 2</th>
						</tr>
					</thead>
					<tbody>
						<tr v-for="(value, index) in getRows[1]" :key="index">
							<td
								:class="[
									'xm__txtAlign-center',
									{ is__firstMatch: getRows[0].includes(value) },
									{ is__secondMatch: getRows[2].includes(value) },
								]"
								:title="
									(getRows[0].includes(value)
										? ` Tabla 1, fila ${getRows[0].indexOf(value) + 1};`
										: '') +
										(getRows[2].includes(value)
											? ` Tabla 3, fila ${getRows[2].indexOf(value) + 1};`
											: '')
								"
							>
								<code>{{ addZeros(value) }}</code>
							</td>
						</tr>
					</tbody>
				</table>
				<table v-if="getRows[2]" class="x-table">
					<thead>
						<tr>
							<th class="xm__txtAlign-center" colspan="50">Tabla 3</th>
						</tr>
					</thead>
					<tbody>
						<tr v-for="(value, index) in getRows[2]" :key="index">
							<td
								:class="[
									'xm__txtAlign-center',
									{ is__firstMatch: getRows[1].includes(value) },
									{ is__secondMatch: getRows[0].includes(value) },
								]"
								:title="
									(getRows[1].includes(value)
										? ` Tabla 2, fila ${getRows[1].indexOf(value) + 1};`
										: '') +
										(getRows[0].includes(value)
											? ` Tabla 1, fila ${getRows[0].indexOf(value) + 1};`
											: '')
								"
							>
								<code>{{ addZeros(value) }}</code>
							</td>
						</tr>
					</tbody>
				</table>
			</div>
			<div
				v-if="singleMatches.length"
				id="two"
				class="x-txt xm__txtAlign-center xm__gaping-5"
			>
				<p>
					<b>Coincidencias de 2 tablas:</b>
					{{ singleMatches.map(n => addZeros(n)).join(", ") }}
				</p>
			</div>
			<div
				v-if="multipleMatches.length"
				id="three"
				class="x-txt xm__txtAlign-center xm__gaping-5"
			>
				<p>
					<b>Coincidencias de 3 tablas:</b>
					{{ multipleMatches.map(n => addZeros(n)).join(", ") }}
				</p>
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
				activeRowsIndex: -1,
			};
		},
		computed: {
			getRows() {
				return this.rows[this.activeRowsIndex] || [];
			},
			singleMatches() {
				// this.getRows.forEach(row => {});
				const matches = [];
				this.getRows[0].forEach(number => {
					if (
						(this.getRows[1].includes(number) || this.getRows[2].includes(number)) &&
						!matches.includes(number)
					) {
						matches.push(number);
					}
				});
				return matches;
			},
			multipleMatches() {
				const matches = [];
				this.getRows[0].forEach(number => {
					if (
						this.getRows[1].includes(number) &&
						this.getRows[2].includes(number) &&
						!matches.includes(number)
					) {
						matches.push(number);
					}
				});
				return matches;
			},
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
				for (let t = 0; t < newRows.length; t++) {
					for (let n = 0; n < 100; n++) {
						newRows[t].push(this.getRandomNumber(newRows[t]));
					}
				}
				this.rows.push(newRows);
				this.activeRowsIndex = this.rows.length - 1;
			},
			goBack() {
				if (this.activeRowsIndex > 0) this.activeRowsIndex--;
			},
			goForward() {
				if (this.activeRowsIndex < this.rows.length - 1) this.activeRowsIndex++;
			},
		},
	};
</script>

<style lang="scss">
	@import "@xamu-co/styles/src/utils/module.scss";
	code {
		color: color(dark);
	}
	.is__firstMatch:not(.is__secondMatch),
	.is__secondMatch:not(.is__firstMatch) {
		// 1 match only: yellow
		background: color(primary);
	}
	.is__firstMatch.is__secondMatch {
		// 2 matches: red
		background: color(success);
	}

	.is__firstMatch:not(.is__secondMatch),
	.is__secondMatch:not(.is__firstMatch),
	.is__firstMatch.is__secondMatch {
		code {
			color: color(light);
			font-weight: bold;
		}
	}

	.u-floating {
		position: fixed;
		bottom: 2rem;
		right: 2rem;
		z-index: 10;
	}
</style>
