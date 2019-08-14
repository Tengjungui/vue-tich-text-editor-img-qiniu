<template>
    <div>
        <div>
            <quill-editor v-model="editorContent" ref="myQuillEditor" :options="editorOption"/>
            <!-- 图片上传 input-->
            <input type="file" id="inputUpload" @change="uploadImg" accept="image/*" v-show="false">
        </div>
        <button class="btn-submit" @click="submitHandle">提交</button>
    </div>
</template>

<script>
import 'quill/dist/quill.core.css';
import 'quill/dist/quill.snow.css';
import 'quill/dist/quill.bubble.css';
import { quillEditor } from 'vue-quill-editor';

// toolbar配置
const toolbarOptions = [
	[{ header: [1, 2, 3, 4, 5, 6, false] }],
	['bold', 'italic'],
	[{ direction: 'rtl' }], // 文字方向
	[{ list: 'ordered' }, { list: 'bullet' }],
	['link', 'image']
];

// 比较全面的配置
// const toolbarOptions = [
// 	['bold', 'italic', 'underline', 'strike'], // 加粗 斜体 下划线 删除线
// 	['blockquote', 'code-block'],

// 	[{ header: 1 }, { header: 2 }], // 自定义按钮值
// 	[{ list: 'ordered' }, { list: 'bullet' }],
// 	[{ script: 'sub' }, { script: 'super' }], // 下标，上标
// 	[{ indent: '-1' }, { ndent: '+1' }], //减少缩进/缩进
// 	[{ direction: 'rtl' }], // 文字方向
// 	[{ size: ['small', false, 'large', 'huge'] }], // 自定义下拉列表
// 	[{ header: [1, 2, 3, 4, 5, 6, false] }],

// 	[{ color: [] }, { background: [] }], // 默认来自主题的下拉列表
// 	[{ font: [] }],
// 	[{ align: [] }],
// 	['link', 'image', 'video'],
// 	['clean'] // 删除格式化按钮
// ];
export default {
	components: {
		'quill-editor': quillEditor
	},
	props: {
		qiniuObj: {
			url: String,
			domain: String,
			token: String
		}
	},
	data() {
		return {
			editorContent: '',
			editorOption: {
				placeholder: '这里输入内容...',
				modules: {
					toolbar: {
						container: toolbarOptions, // 工具栏
						handlers: {
							image: function(val) {
								if (val) {
									document.querySelector('#inputUpload').click();
								} else {
									this.quill.format('image', false);
								}
							}
						}
					}
				}
			}
		};
	},
	computed: {
		editor() {
			return this.$refs.myQuillEditor.quill;
		}
	},
	mounted() {},
	created() {},
	methods: {
		uploadImg(e) {
			const file = e.target.files[0];
			this.uploadImgToQiniu(file);
		},

		//上传图片到七牛
		uploadImgToQiniu(file) {
			let data = new FormData();
			let suffixName =
				'zx/' + +new Date() + '' + ((Math.random() * 1e3) | 0) + file.name.slice(file.name.lastIndexOf('.'));
			data.append('token', this.qiniuObj.token); //七牛需要的token
			data.append('file', file);
			data.append('key', suffixName); //后缀名
			this.axios({
				method: 'POST',
				url: this.qiniuObj.url, //七牛域名
				data: data,
				timeout: 30000, //超时
				onUploadProgress: res => {
					//上传进度
					let imgUploadProgress = Math.round((res.loaded * 100) / res.total);
					console.log('当前进度：', imgUploadProgress);
				}
			}).then(res => {
				document.getElementById('inputUpload').value = ''; //上传成功后，input valuel置空
				let imgUrls = `${this.qiniuObj.domain}/${res.data.key}`; //拼接图片的地址
				this.uploadSuccess(imgUrls);
			});
		},

		uploadSuccess(res) {
			let quill = this.$refs.myQuillEditor.quill; // 获取富文本组件实例
			let length = quill.getSelection().index; // 获取光标所在位置
			quill.insertEmbed(length, 'image', res); // 插入图片  res为服务器返回的图片地址
			quill.setSelection(length + 1); // 调整光标到最后
		},

		//提交
		submitHandle() {
			if (!this.editorContent) {
				return confirm('请输入内容！');
			}
			console.log(this.editorContent);
			alert('提交成功！谢谢~');
		}
	}
};
</script>

<style lang="less">
.ql-toolbar.ql-snow + .ql-container.ql-snow {
	height: calc(100vh - 120px);
}
.ql-toolbar.ql-snow {
	background: #e4e7ed;
	border: 0;
}
.ql-container.ql-snow {
	border: 0;
}
.btn-submit {
	display: block;
	margin: 5px auto;
	width: 90%;
	line-height: 40px;
	border: 0;
	background-color: #409eff;
	font-size: 20px;
	color: #fff;
	border-radius: 20px;
	outline: 0;
}
</style>
