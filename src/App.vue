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
				url: 'https://upload-z2.qiniup.com/', //七牛域名
				domain: 'https://image.lingyiliebian.com', //服务器子域名
				token:
					'jST00mAfYB_6B8eZgg1P504PGRdIJpvuaDnRCviM:lftzOm5_ZJrra9tK1aS7HGk-9Iw=:eyJzY29wZSI6ImltYWdlcyIsImRlYWRsaW5lIjoxNTY1NzcxNjA0fQ==' //七牛token
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