<template>
  <div ref="cm-editor"></div>
</template>

<script lang="ts">
import { Vue, Component, Prop, Ref } from 'vue-property-decorator';
import * as CodeMirror from 'codemirror';
import 'codemirror/lib/codemirror.css';
import 'codemirror/addon/merge/merge.css';

require('../../../../third_party/js/diff_match_patch.js');
require('codemirror/addon/merge/merge.js');

@Component
export default class DiffView extends Vue {
  @Prop() left!: string;
  @Prop() right!: string;
  @Ref('cm-editor') private readonly cmEditor!: HTMLElement;

  private editor: CodeMirror.MergeView.MergeViewEditor | null = null;

  mounted() {
    if (this.editor) return;
    this.editor = CodeMirror.MergeView(this.cmEditor, {
      collapseIdentical: true,
      connect: 'align',
      lineNumbers: true,
      mode: 'text/plain',
      orig: this.right,
      origRight: this.left,
      readOnly: true,
      revertButtons: false,
      showDifferences: true,
      tabSize: 2,
      value: this.right,
    });
  }
}
</script>
