<template>
  <BlocklyComponent class="absolute top-0 left-0 w-3/4 h-full" :options="options" ref="foo"></BlocklyComponent>

  <div class="absolute top-0 right-0 w-1/4">
    <div class="cursor-pointer">
      <img
        src="./assets/placeholder-4-3.png"
        alt="Video Feed"
      >
    </div>

    <div class="p-3">
      <button
        class="block w-full px-4 py-2 rounded-lg transition duration-100 ease-in-out focus:border-blue-500 focus:ring-2 focus:ring-blue-500 focus:outline-none focus:ring-opacity-50 disabled:opacity-50 disabled:cursor-not-allowed text-white bg-blue-500 border border-transparent shadow-sm hover:bg-blue-600"
        @click="publish"
      >
        Publicera
      </button>
    </div>
  </div>
</template>

<script>
/**
 * @license
 * 
 * Copyright 2019 Google LLC
 *
 * Licensed under the Apache License, Version 2.0 (the "License");
 * you may not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 *
 *   http://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License.
 */

/**
 * @fileoverview Main Vue component that includes the Blockly component.
 * @author samelh@google.com (Sam El-Husseini)
 */

import BlocklyComponent from './components/BlocklyComponent.vue'
import './prompt'

import BlocklyPython from 'blockly/python'

import axios from 'axios'

export default {
  name: 'app',
  components: {
    BlocklyComponent
  },
  data(){
    return {
      options: {
        media: './assets/media',
        grid:
          {
            spacing: 25,
            length: 3,
            colour: '#ccc',
            snap: true
          },
        toolbox:
        `<xml>
          <category name="Logic" colour="%{BKY_LOGIC_HUE}">
            <block type="controls_if"></block>
            <block type="logic_compare"></block>
            <block type="logic_operation"></block>
            <block type="logic_negate"></block>
            <block type="logic_boolean"></block>
          </category>
          <category name="Loops" colour="%{BKY_LOOPS_HUE}">
            <block type="controls_repeat_ext">
              <value name="TIMES">
                <block type="math_number">
                  <field name="NUM">10</field>
                </block>
              </value>
            </block>
            <block type="controls_whileUntil"></block>
          </category>
          <category name="Math" colour="%{BKY_MATH_HUE}">
            <block type="math_number">
              <field name="NUM">123</field>
            </block>
            <block type="math_arithmetic"></block>
            <block type="math_single"></block>
          </category>
          <category name="Text" colour="%{BKY_TEXTS_HUE}">
            <block type="text"></block>
            <block type="text_length"></block>
            <block type="text_print"></block>
          </category>
          <category name="Variables" custom="VARIABLE" colour="%{BKY_VARIABLES_HUE}">
            </category>
        </xml>`,
        fullscreen: false
      }
    }
  },
  methods: {
    publish () {
      const code = BlocklyPython.workspaceToCode(this.$refs["foo"].workspace)

      axios.post('http://localhost:4000', {
        code
      })
      .then(function (response) {
        console.log(response)
      })
      .catch(function (error) {
        console.log(error)
      })
    }
  }
}
</script>
