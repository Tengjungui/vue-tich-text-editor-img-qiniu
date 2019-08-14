<template>
    <div id="app">
        <tich-text-editor :qiniuObj="qiniuObj"/>
    </div>
</template>
<script>
import TichTextEditor from './components/tichTextEditor';
export default {
	name: 'app',
	components: {
		TichTextEditor
	},
	data() {
		return {
			qiniuObj: {
				url: '', //七牛域名
				domain: '', //服务器子域名
				token: '' //七牛token
			}
		};
	}
};
</script>
<style lang="less">
* {
	padding: 0;
	margin: 0;
}
</style>