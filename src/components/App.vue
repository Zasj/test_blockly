<script lang="ts" setup>
/**
 * @license
 * Copyright 2022 Google LLC
 * SPDX-License-Identifier: Apache-2.0
 */

/**
 * @fileoverview Main Vue component that includes the Blockly component.
 * @author dcoodien@google.com (Dylan Coodien)
 */

import type { AppContext } from "@netless/window-manager";

import { computed, inject, onMounted, ref, watchEffect } from "vue";
import BlocklyComponent from "./components/BlocklyComponent.vue";
import "./blocks/stocks";

import BlocklyJS from "blockly/javascript";
import BlocklyPY from "blockly/python"
import BlocklyPHP from "blockly/php"
import BlocklyLua from "blockly/lua"
import BlocklyDART from "blockly/dart"

// const foo = ref();
// const code = ref();
const options = {
  media: "node_modules/blockly/media/",
  grid: {
    spacing: 25,
    length: 3,
    colour: "#ccc",
    snap: true,
  },
  toolbox: `<xml>
		<category name="逻辑" colour="%{BKY_LOGIC_HUE}">
		  <block type="controls_if"></block>
		  <block type="logic_compare"></block>
		  <block type="logic_operation"></block>
		  <block type="logic_negate"></block>
		  <block type="logic_boolean"></block>
		  <block type="logic_null"></block>
		  <block type="logic_ternary"></block>
		</category>
		<category name="循环" colour="%{BKY_LOOPS_HUE}">
		  <block type="controls_repeat_ext">
			<value name="TIMES">
			  <shadow type="math_number">
				<field name="NUM">10</field>
			  </shadow>
			</value>
		  </block>
		  <block type="controls_whileUntil"></block>
		  <block type="controls_for">
			<value name="FROM">
			  <shadow type="math_number">
				<field name="NUM">1</field>
			  </shadow>
			</value>
			<value name="TO">
			  <shadow type="math_number">
				<field name="NUM">10</field>
			  </shadow>
			</value>
			<value name="BY">
			  <shadow type="math_number">
				<field name="NUM">1</field>
			  </shadow>
			</value>
		  </block>
		  <block type="controls_forEach"></block>
		  <block type="controls_flow_statements"></block>
		</category>
		<category name="数学" colour="%{BKY_MATH_HUE}">
		  <block type="math_number">
			<field name="NUM">123</field>
		  </block>
		  <block type="math_arithmetic">
			<value name="A">
			  <shadow type="math_number">
				<field name="NUM">1</field>
			  </shadow>
			</value>
			<value name="B">
			  <shadow type="math_number">
				<field name="NUM">1</field>
			  </shadow>
			</value>
		  </block>
		  <block type="math_single">
			<value name="NUM">
			  <shadow type="math_number">
				<field name="NUM">9</field>
			  </shadow>
			</value>
		  </block>
		  <block type="math_trig">
			<value name="NUM">
			  <shadow type="math_number">
				<field name="NUM">45</field>
			  </shadow>
			</value>
		  </block>
		  <block type="math_constant"></block>
		  <block type="math_number_property">
			<value name="NUMBER_TO_CHECK">
			  <shadow type="math_number">
				<field name="NUM">0</field>
			  </shadow>
			</value>
		  </block>
		  <block type="math_round">
			<value name="NUM">
			  <shadow type="math_number">
				<field name="NUM">3.1</field>
			  </shadow>
			</value>
		  </block>
		  <block type="math_on_list"></block>
		  <block type="math_modulo">
			<value name="DIVIDEND">
			  <shadow type="math_number">
				<field name="NUM">64</field>
			  </shadow>
			</value>
			<value name="DIVISOR">
			  <shadow type="math_number">
				<field name="NUM">10</field>
			  </shadow>
			</value>
		  </block>
		  <block type="math_constrain">
			<value name="VALUE">
			  <shadow type="math_number">
				<field name="NUM">50</field>
			  </shadow>
			</value>
			<value name="LOW">
			  <shadow type="math_number">
				<field name="NUM">1</field>
			  </shadow>
			</value>
			<value name="HIGH">
			  <shadow type="math_number">
				<field name="NUM">100</field>
			  </shadow>
			</value>
		  </block>
		  <block type="math_random_int">
			<value name="FROM">
			  <shadow type="math_number">
				<field name="NUM">1</field>
			  </shadow>
			</value>
			<value name="TO">
			  <shadow type="math_number">
				<field name="NUM">100</field>
			  </shadow>
			</value>
		  </block>
		  <block type="math_random_float"></block>
		  <block type="math_atan2">
			<value name="X">
			  <shadow type="math_number">
				<field name="NUM">1</field>
			  </shadow>
			</value>
			<value name="Y">
			  <shadow type="math_number">
				<field name="NUM">1</field>
			  </shadow>
			</value>
		  </block>
		</category>
		<category name="文本" colour="%{BKY_TEXTS_HUE}">
		  <block type="text"></block>
		  <block type="text_join"></block>
		  <block type="text_append">
			<value name="TEXT">
			  <shadow type="text"></shadow>
			</value>
		  </block>
		  <block type="text_length">
			<value name="VALUE">
			  <shadow type="text">
				<field name="TEXT">abc</field>
			  </shadow>
			</value>
		  </block>
		  <block type="text_isEmpty">
			<value name="VALUE">
			  <shadow type="text">
				<field name="TEXT"></field>
			  </shadow>
			</value>
		  </block>
		  <block type="text_indexOf">
			<value name="VALUE">
			  <block type="variables_get">
				<field name="VAR">{textVariable}</field>
			  </block>
			</value>
			<value name="FIND">
			  <shadow type="text">
				<field name="TEXT">abc</field>
			  </shadow>
			</value>
		  </block>
		  <block type="text_charAt">
			<value name="VALUE">
			  <block type="variables_get">
				<field name="VAR">{textVariable}</field>
			  </block>
			</value>
		  </block>
		  <block type="text_getSubstring">
			<value name="STRING">
			  <block type="variables_get">
				<field name="VAR">{textVariable}</field>
			  </block>
			</value>
		  </block>
		  <block type="text_changeCase">
			<value name="TEXT">
			  <shadow type="text">
				<field name="TEXT">abc</field>
			  </shadow>
			</value>
		  </block>
		  <block type="text_trim">
			<value name="TEXT">
			  <shadow type="text">
				<field name="TEXT">abc</field>
			  </shadow>
			</value>
		  </block>
		  <block type="text_print">
			<value name="TEXT">
			  <shadow type="text">
				<field name="TEXT">abc</field>
			  </shadow>
			</value>
		  </block>
		  <block type="text_prompt_ext">
			<value name="TEXT">
			  <shadow type="text">
				<field name="TEXT">abc</field>
			  </shadow>
			</value>
		  </block>
		</category>
		<category name="列表" colour="%{BKY_LISTS_HUE}">
		  <block type="lists_create_with">
			<mutation items="0"></mutation>
		  </block>
		  <block type="lists_create_with"></block>
		  <block type="lists_repeat">
			<value name="NUM">
			  <shadow type="math_number">
				<field name="NUM">5</field>
			  </shadow>
			</value>
		  </block>
		  <block type="lists_length"></block>
		  <block type="lists_isEmpty"></block>
		  <block type="lists_indexOf">
			<value name="VALUE">
			  <block type="variables_get">
				<field name="VAR">{listVariable}</field>
			  </block>
			</value>
		  </block>
		  <block type="lists_getIndex">
			<value name="VALUE">
			  <block type="variables_get">
				<field name="VAR">{listVariable}</field>
			  </block>
			</value>
		  </block>
		  <block type="lists_setIndex">
			<value name="LIST">
			  <block type="variables_get">
				<field name="VAR">{listVariable}</field>
			  </block>
			</value>
		  </block>
		  <block type="lists_getSublist">
			<value name="LIST">
			  <block type="variables_get">
				<field name="VAR">{listVariable}</field>
			  </block>
			</value>
		  </block>
		  <block type="lists_split">
			<value name="DELIM">
			  <shadow type="text">
				<field name="TEXT">,</field>
			  </shadow>
			</value>
		  </block>
		  <block type="lists_sort"></block>
		</category>
		<sep></sep>
		<category name="变量" colour="%{BKY_VARIABLES_HUE}" custom="VARIABLE"></category>
		<category name="函数" colour="%{BKY_PROCEDURES_HUE}" custom="PROCEDURE"></category>
	  </xml>`,
};


const context = inject<AppContext>("context");
if (!context) throw new Error("must call provide('context') before mount App");

interface ws {
	foo: any;
	code: any;
}

const ws: ws = {
	foo: null,
	code: "",
}

const storage = context.createStorage("wser", ws);

const data: any = {
	foo: ref(storage.state.foo),
	code: ref(storage.state.code),
}

const foo = computed<any>({
  get: () => data.foo.value,
  set: (foo) => storage.setState({ foo }),
});

const code = computed<any>({
  get: () => data.code.value,
  set: (code) => storage.setState({ code }),
});

onMounted(() =>
  storage.addStateChangedListener(() => {
    data.foo.value = storage.state.foo;
	data.code.value = storage.state.code;
	// console.log(foo.value);
  })
);
watchEffect(() => {
  console.log("App.vue: code =", code.value);
  console.log("App.vue: foo =",foo.value);
});

const showCode_js = () => (code.value = BlocklyJS.workspaceToCode(foo.value.workspace));
const showCode_py = () => (code.value = BlocklyPY.workspaceToCode(foo.value.workspace));
const showCode_php = () => (code.value = BlocklyPHP.workspaceToCode(foo.value.workspace));
const showCode_lua = () => (code.value = BlocklyLua.workspaceToCode(foo.value.workspace));
const showCode_dart = () => (code.value = BlocklyDART.workspaceToCode(foo.value.workspace));
</script>

<template>
  <div id="app">
    <BlocklyComponent id="blockly" :options="options" ref="foo"></BlocklyComponent>
    <p id="code">
      <!-- <button v-on:click="showCode()" ></button> -->
	  <button style="border: 0;background: 0%;">
		  <img src="./assets/js.png" height="25" width="25" v-on:click="showCode_js()" title="生成JavaScript代码"/>
	  </button>
	  <button style="border: 0;background: 0%;">
	  		  <img src="./assets/python3.png" height="25" width="25" v-on:click="showCode_py()" title="生成Python代码"/>
	  </button>
	  <button style="border: 0;background: 0%;">
	  		  <img src="./assets/php.png" height="25" width="25" v-on:click="showCode_php()" title="生成php代码"/>
	  </button>
	  <button style="border: 0;background: 0%;">
	  		  <img src="./assets/lua.png" height="25" width="25" v-on:click="showCode_lua()" title="生成Lua代码"/>
	  </button>
	  <button style="border: 0;background: 0%;">
	  		  <img src="./assets/dart.png" height="25" width="25" v-on:click="showCode_dart()" title="生成Dart代码"/>
	  </button>
      <pre v-html="code"></pre>
    </p>
  </div>
</template>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}

html,
body {
  margin: 0;
}

#code {
  position: absolute;
  right: 0;
  bottom: 0;
  width: 35%;
  height: 100%;
  margin: 0;
  background-color: beige;
}

#blockly {
  position: absolute;
  left: 0;
  bottom: 0;
  width: 65%;
  height: 100%;
}

/* Buttons */
button {
  margin: 5px;
  padding: 10px;
  border-radius: 4px;
  border: 1px solid #ddd;
  font-size: large;
  background-color: #eee;
  color: #000;
}
button.primary {
  border: 1px solid #dd4b39;
  background-color: #dd4b39;
  color: #fff;
}
button.primary>img {
  opacity: 1;
}
button>img {
  opacity: 0.6;
  vertical-align: text-bottom;
}
button:hover>img {
  opacity: 1;
}

button:hover {
  box-shadow: 2px 2px 5px #888;
}
button.disabled:hover>img {
  opacity: 0.6;
}
button.disabled {
  display: none;
}
button.notext {
  font-size: 10%;
}

/* Buttons */
button {
  padding: 1px 10px;
  margin: 1px 5px;
}
</style>
