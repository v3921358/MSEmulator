﻿
<template>
	<div class="UIEditCharacterAttribute" style="display: inline-block;">
		<div style="display: inline-block; vertical-align: top;">
			<table>
				<tr>
					<td><div>
						<button v-if="chara.speed" @click="pauseAnimation" title="pause animation" class="btn"><img src="/images/player_pause.png" alt="pause"></button>
						<button v-else @click="pauseAnimation" title="play animation" class="btn"><img src="/images/player_play.png" alt="play"></button>
					</div></td>
					<td>
						<button v-if="sceneChara.enablePhysics" @click="sceneChara.enablePhysics=false">disable</button>
						<button v-else @click="sceneChara.enablePhysics=true">enable</button>
					</td>
					<td><button @click="isShowDebug=!isShowDebug">detail</button></td>
				</tr>
				<tr>
					<td><label for="chara_action">action</label></td>
					<td><select id="chara_action" v-model="chara.action" @clicl="update_frame_list('action')">
							<option v-if="!actions.length" disabled value="">请选择</option>
							<option v-else v-for="item in actions">{{item}}</option>
						</select></td>
					<td>
						<select v-model.number="chara.action_frame">
							<option v-if="!actions.length" disabled> ---- </option>
							<option v-else v-for="frame in chara.action_frame_count" :value="frame - 1">{{frame - 1}}</option>
						</select>
					</td>
				</tr>
				<tr>
					<td><label for="chara_emotion">emotion</label></td>
					<td><select id="chara_emotion" v-model="chara.emotion">
							<option v-if="!emotions.length" disabled value="">请选择</option>
							<option v-else v-for="item in emotions">{{item}}</option>
						</select></td>
					<td>
						<select v-model.number="chara.emotion_frame" @clicl="update_frame_list('emotion')">
							<option v-if="!emotions.length" disabled> ---- </option>
							<option v-else v-for="frame in chara.emotion_frame_count" :value="frame - 1">{{frame - 1}}</option>
						</select>
					</td>
				</tr>
				<tr>
					<td>location</td>
					<td colspan="2">
						<input type="number" v-model.number="chara.x" min="-9999" max="9999" style="width: 5em;" />
						<input type="number" v-model.number="chara.y" min="-9999" max="9999" style="width: 5em;" />
						<input type="number" v-model.number="sceneChara.$layer" min="0"     max="7"    style="width: 5em;" />
					</td>
				</tr>
				<tr>
					<td>
						<label class="chara_ear">elf<input type="radio" name="chara_ear" v-model="chara.ear" value="elfEar" /></label>
					</td>
					<td>
						<label class="chara_ear">human<input type="radio" name="chara_ear" v-model="chara.ear" value="human" checked /></label>
					</td>
					<td>
						<label class="chara_ear">雷普<input type="radio" name="chara_ear" v-model="chara.ear" value="lefEar" /></label>
					</td>
				</tr>
				<!--<tr>
					<td><button @click="chara._download()">download</button></td>
					<td></td>
					<td></td>
				</tr>
				<tr>
					<td></td>
					<td></td>
					<td></td>
				</tr>-->
			</table>
		</div>
		<div v-if="isShowDebug" style="display: inline-block; user-select: text;">
			<ul>
				<template v-for="equip in chara.slots._ordered_slot" v-if="equip && isEquip(equip.id)">
					<li>
						<div style="display: inline-block; width: 32px; height: 32px;">
							<img :src="'images/' + equip._url + 'info/iconRaw'" />
						</div>
						<div style="display: inline-block;">
							<div>{{equip.name}}</div>
							<div>{{equip.id}}</div>
						</div>
					</li>
				</template>
			</ul>
		</div>
	</div>
</template>

<script>
	import { ItemCategoryInfo } from "../../public/resource.js";

	export default {
		props: ["sceneChara"],
		data: function () {
			return {
				isShowDebug: false,
			}
		},
		computed: {
			actions: () => character_action_list,
			emotions: () => character_emotion_list,
			chara: function () {
				return this.sceneChara.renderer;
			},
		},
		methods: {
			pauseAnimation: function () {
				this.chara.speed = this.chara.speed ? 0 : 1;//this.pause ? 0 : 1;
			},
			isEquip: function (id) {
				return ItemCategoryInfo.isEquip(id);
			},
		},
	}

</script>

<style scoped>
	.UIEditCharacterAttribute td > * {
		width: 100%;
	}
	
	.btn {
		display: inline-flex;
		width: 20px;
		height: 20px;
		padding: 0;
	}
	.chara_ear {
		width: 100%;
		display: block;
	}
</style>
