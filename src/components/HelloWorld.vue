<template>
	<div class="print_template_container">
		<el-container class="print_template_main" style="height: 100vh;">
			<el-aside width="240px">
				<el-row>
					<el-col :span="24" class="rect-printElement-types hiprintEpContainer">
						<el-row class="drag_item_title">拖拽组件列表</el-row>
						<el-row style="height: 100px;">
							<el-col :span="12" class="drag_item_box">
								<div>
									<a class="ep-draggable-item" tid="defaultModule.text" style>
										<span class="glyphicon glyphicon-text-width" aria-hidden="true"></span>
										<p class="glyphicon-class">文本</p>
									</a>
								</div>
							</el-col>
							<el-col :span="12" class="drag_item_box" tid="defaultModule.text">
								<div>
									<a class="ep-draggable-item" tid="defaultModule.image" style>
										<span class="glyphicon glyphicon-picture" aria-hidden="true"></span>
										<p class="glyphicon-class">图片</p>
									</a>
								</div>
							</el-col>
						</el-row>
						<el-row style="height: 100px;">
							<el-col :span="12" class="drag_item_box" tid="defaultModule.text">
								<div>
									<a class="ep-draggable-item" tid="defaultModule.longText">
										<span class="glyphicon glyphicon-subscript" aria-hidden="true"></span>
										<p class="glyphicon-class">长文</p>
									</a>
								</div>
							</el-col>
							<el-col :span="12" class="drag_item_box" tid="defaultModule.text">
								<div>
									<a class="ep-draggable-item" tid="defaultModule.tableCustom" style>
										<span class="glyphicon glyphicon-th" aria-hidden="true"></span>
										<p class="glyphicon-class">表格</p>
									</a>
								</div>
							</el-col>
						</el-row>
						<el-row class="drag_item_title">辅助</el-row>
						<el-row style="height: 100px;">
							<el-col :span="12" class="drag_item_box" tid="defaultModule.text">
								<div>
									<a class="ep-draggable-item" tid="defaultModule.hline" style>
										<span class="glyphicon glyphicon-resize-horizontal" aria-hidden="true"></span>
										<p class="glyphicon-class">横线</p>
									</a>
								</div>
							</el-col>
							<el-col :span="12" class="drag_item_box" tid="defaultModule.text">
								<div>
									<a class="ep-draggable-item" tid="defaultModule.vline" style>
										<span class="glyphicon glyphicon-resize-vertical" aria-hidden="true"></span>
										<p class="glyphicon-class">竖线</p>
									</a>
								</div>
							</el-col>
						</el-row>
						<el-row style="height: 100px;">
							<el-col :span="12" class="drag_item_box" tid="defaultModule.text">
								<div>
									<a class="ep-draggable-item" tid="defaultModule.rect">
										<span class="glyphicon glyphicon-unchecked" aria-hidden="true"></span>
										<p class="glyphicon-class">矩形</p>
									</a>
								</div>
							</el-col>
							<el-col :span="12" class="drag_item_box" tid="defaultModule.text">
								<div>
									<a class="ep-draggable-item" tid="defaultModule.oval">
										<span class="glyphicon glyphicon-record" aria-hidden="true"></span>
										<p class="glyphicon-class">椭圆</p>
									</a>
								</div>
							</el-col>
						</el-row>
					</el-col>
				</el-row>
			</el-aside>
			<el-container>
				<el-header>
					<el-row style="height: 100%">
						<el-col :span="24" style="height: 100%">
							<el-button-group>
								<el-button :type="currentPaperType === 'A3' ? 'primary' : 'info'" @click="setPaper('A3')">A3</el-button>
								<el-button :type="currentPaperType === 'A4' ? 'primary' : 'info'" @click="setPaper('A4')">A4</el-button>
								<el-button :type="currentPaperType === 'A5' ? 'primary' : 'info'" @click="setPaper('A5')">A5</el-button>
								<el-button :type="currentPaperType === 'B3' ? 'primary' : 'info'" @click="setPaper('B3')">B3</el-button>
								<el-button :type="currentPaperType === 'B4' ? 'primary' : 'info'" @click="setPaper('B4')">B4</el-button>
								<el-button :type="currentPaperType === 'B5' ? 'primary' : 'info'" @click="setPaper('B5')">B5</el-button>
							</el-button-group>
							<el-popover
								placement="bottom"
								title="请输入自定义纸张宽高"
								trigger="click">
								<el-form ref="otherPaperForm" label-position="left" :model="otherPaper" label-width="30px">
									<el-row>
										<el-col :span="16">
											<el-form-item label="宽">
												<el-input v-model="otherPaper.width">
													<template slot="append">mm</template>
												</el-input>
											</el-form-item>
										</el-col>
									</el-row>
									<el-row>
										<el-col :span="16">
											<el-form-item label="高">
												<el-input v-model="otherPaper.height">
													<template slot="append">mm</template>
												</el-input>
											</el-form-item>
										</el-col>
									</el-row>
									<el-form-item>
										<el-button type="primary" @click="setPaper('other', otherPaper.width, otherPaper.height)">确定</el-button>
										<el-button @click="setPaper('other', '', '')">清空</el-button>
									</el-form-item>
									</el-form>
								<el-button slot="reference" :type="currentPaperType === 'other' ? 'primary' : 'info'">自定义纸张</el-button>
							</el-popover>
							<el-button-group>
								<el-button type="info" icon="el-icon-refresh-right" @click="rotatePaper">旋转</el-button>
								<el-button type="info" icon="el-icon-delete" @click="clearTemplate">清空</el-button>
							</el-button-group>
							<el-button-group>
								<el-button type="info" icon="el-icon-view" @click="previewTemplate()">快速预览</el-button>
								<el-button type="info" icon="el-icon-document" @click="getJson()">预览JSON</el-button>
								<el-button type="info" icon="el-icon-tickets" @click="getHtml(printData)">预览HTML</el-button>
							</el-button-group>
							<el-button-group>
								<el-button
									type="info"
									icon="el-icon-full-screen"
									v-if="!fullDialogStatus"
									@click="fullDialogStatus = true"
								>全屏</el-button>
								<el-button
									type="info"
									icon="el-icon-full-screen"
									v-if="fullDialogStatus"
									@click="fullDialogStatus = false"
								>正常屏幕</el-button>
							</el-button-group>
							<el-button-group>
								<el-button type="info" icon="el-icon-printer" @click="printTemplate(printData)">打印</el-button>
							</el-button-group>
							<el-button-group>
								<el-button type="info" icon="el-icon-coin" @click="bindDataSources">数据源</el-button>
							</el-button-group>
						</el-col>
					</el-row>
				</el-header>
				<el-main>
					<el-row>
						<el-col
							style="padding: 20px"
							:span="18"
							id="hiprint-printTemplate"
							class="hiprint-printTemplate"
						></el-col>
						<el-col :span="6" style="display:none" class="params_setting_container">
							<el-row class="hinnn-layout-sider" style="padding: 10px;">
								<div id="PrintElementOptionSetting"></div>
							</el-row>
						</el-col>
					</el-row>

					<el-drawer
						size="100%"
						class="params_drawer"
						:title="paramsDrawerTitle"
						:modal="false"
						:wrapperClosable="true"
						@opened="handleParamsOpened"
						:visible.sync="paramsDrawerStatus"
						direction="rtl"
					>
						<h4 slot="title">参数设置: {{ paramsDrawerTitle }}</h4>
						<div id="params_content"></div>
					</el-drawer>
				</el-main>
			</el-container>
		</el-container>

		<el-dialog
			class="preview_dialog"
			ref="previewDialog"
			title="快速预览"
			@opened="handlePreviewOpened(printData)"
			:width="currentPaper.width * 1 + 'mm'"
			:visible.sync="previewDialogStatus"
		>
			<div id="preview_content"></div>
			<span slot="footer" class="dialog-footer">
				<el-button @click="previewDialogStatus = false">取 消</el-button>
				<el-button type="primary" @click="previewDialogStatus = false">确 定</el-button>
			</span>
		</el-dialog>

		<el-dialog
			ref="infoDialog"
			:title="infoDialogTitle"
			:lock-scroll="true"
			:visible.sync="infoDialogStatus"
		>
			<el-input
				type="textarea"
				:readonly="true"
				:autosize="{ minRows: 15}"
				placeholder="请输入内容"
				v-model="infoTextarea"
			></el-input>
			<span slot="footer" class="dialog-footer">
				<el-button @click="infoDialogStatus = false">取 消</el-button>
				<el-button type="primary" @click="infoDialogStatus = false">复 制</el-button>
			</span>
		</el-dialog>

		<el-dialog ref="infoDialog" title="数据源绑定" :lock-scroll="true" :visible.sync="dataDialogStatus">
			<el-table :data="dataSources" style="width: 100%">
				<el-table-column label="组件" width="100">
					<template slot-scope="scope">
						{{ scope.row.printElementType.title }}
					</template>
				</el-table-column>
				<el-table-column label="类型" width="100">
					<template slot-scope="scope">
						{{ scope.row.printElementType.type }}
					</template>
				</el-table-column>
				<el-table-column label="字段" width="100">
					<template slot-scope="scope">
						{{ scope.row.options.field }}
					</template>
				</el-table-column>
				<el-table-column label="标题">
					<template slot-scope="scope">
						{{ scope.row.options.title }}
					</template>
				</el-table-column>
				<el-table-column label="远程数据源">
					<template slot-scope="scope">
						<el-select :value="scope.row.options.field" placeholder="请选择">
							<el-option
							:label="'名称数据'"
							:value="'name'">
							</el-option>
							<el-option
							:label="'描述数据'"
							:value="'desc'">
							</el-option>
						</el-select>
					</template>
				</el-table-column>
			</el-table>
			<span slot="footer" class="dialog-footer">
				<el-button @click="dataDialogStatus = false">取 消</el-button>
				<el-button type="primary" @click="dataDialogStatus = false">复 制</el-button>
			</span>
		</el-dialog>

		<el-dialog
			class="full_screen_dialog"
			:show-close="false"
			:visible.sync="fullDialogStatus"
			:modal="false"
			@opened="openFullScreen"
			@close="closeFullScreen"
			:fullscreen="true"
		>
			<div class="full_content_container" style="width:100%; height: 100%"></div>
		</el-dialog>
	</div>
</template>
<script>
import '@/assets/plugins/hiprint/css/hiprint.css'
import '@/assets/plugins/hiprint/css/print-lock.css'

import '@/assets/plugins/hiprint/polyfill.min.js'
import '@/assets/plugins/hiprint/plugins/jquery.minicolors.min.js'
import { hiprint } from '@/assets/plugins/hiprint/hiprint.bundle.js'
import '@/assets/plugins/hiprint/plugins/jquery.hiwprint.js'
import '@/assets/plugins/hiprint/plugins/qrcode.js'
import '@/assets/plugins/hiprint/plugins/JsBarcode.all.min.js'
import { defaultElementTypeProvider } from '@/assets/plugins/hiprint/etypes/default-etype-provider.js'
import { printData } from '@/assets/plugins/hiprint/custom_test/print-data.js'
import { customPrintJson } from '@/assets/plugins/hiprint/custom_test/custom-print-json.js'
import { setTimeout } from 'timers';
var hiprintTemplate;
export default {
	data() {
		return {
			printData: printData,
			customPrintJson: customPrintJson,
			previewDialogStatus: false,
			paramsDrawerStatus: false,
			infoDialogStatus: false,
			fullDialogStatus: false,
			dataDialogStatus: false,
			infoTextarea: '',
			infoDialogTitle: '',
			paramsDrawerTitle: '',
			previewHtml: null,
			paperMap: {
				A3: {
					width: 420,
					height: 296.6
				},
				A4: {
					width: 210,
					height: 296.6
				},
				A5: {
					width: 210,
					height: 147.6
				},
				B3: {
					width: 500,
					height: 352.6
				},
				B4: {
					width: 250,
					height: 352.6
				},
				B5: {
					width: 250,
					height: 175.6
				}
			},
			currentPaper: {
				width: null,
				height: null
			},
			otherPaper: {
				width: '',
				height: ''
			},
			dataSources: []
		}
	},
	computed: {
		/**
		 * @Author: TangLiangcheng
		 * @Date: 2019-07-30 16:06:13
		 * @Desc: 处理当前选中纸张
		 */
		currentPaperType() {
			let type = null;
			if (this.otherPaper.width !== '' && this.otherPaper.height !== '') {
				type = 'other'
			} else {
				for (const key in this.paperMap) {
					let item = this.paperMap[key]
					let { width, height } = this.currentPaper
					if (item.width === width && item.height === height) {
						type = key
					}
				}
			}

			return type
		}
	},
	methods: {
		/**
		 * @Author: TangLiangcheng
		 * @Date: 2019-07-30 10:19:43
		 * @Desc: 设置纸张属性
		 * @param width 当前纸张宽度 mm
		 * @param height 当前纸张高度 mm
		 */
		setCurrentPaper(obj) {
			let { width, height } = obj;
			this.currentPaper.width = width;
			this.currentPaper.height = height;
		},
		/**
		 * @Author: TangLiangcheng
		 * @Date: 2019-07-30 09:35:36
		 * @Desc: 设置纸张
		 * @param type [A3, A4, A5, B3, B4, B5, other]
		 * @param width 自定义高度
		 * @param height 自定义高度
		 */
		setPaper(type, width, height) {
			try {
				if (type === 'other') {
					if (width === '' && height === '') {
						hiprintTemplate.setPaper('A4', null);
						this.setCurrentPaper(this.paperMap['A4'])
						this.otherPaper = {width: '', height: ''}
					} else {
						hiprintTemplate.setPaper(width, height);
						this.setCurrentPaper({ width: width, height: height })
					}
				} else {
					hiprintTemplate.setPaper(type, null);
					this.setCurrentPaper(this.paperMap[type])
					this.otherPaper = {width: '', height: ''}
				}
			} catch (error) {
				this.$message({
					message: '操作失败:' + error,
					type: 'error'
				});
			}
		},
		/**
		 * @Author: TangLiangcheng
		 * @Date: 2019-07-30 09:41:02
		 * @Desc: 旋转
		 */
		rotatePaper() {
			try {
				hiprintTemplate.rotatePaper();
			} catch (error) {
				this.$message({
					message: '操作失败:' + error,
					type: 'error'
				});
			}
		},
		/**
		 * @Author: TangLiangcheng
		 * @Date: 2019-07-30 09:41:38
		 * @Desc: 清除配置
		 */
		clearTemplate() {
			try {
				hiprintTemplate.clear();
			} catch (error) {
				this.$message({
					message: '操作失败:' + error,
					type: 'error'
				});
			}
		},
		/**
		 * @Author: TangLiangcheng
		 * @Date: 2019-07-30 09:56:28
		 * @Desc: 快速预览
		 */
		previewTemplate() {
			this.previewDialogStatus = true;
		},
		/**
		 * @Author: TangLiangcheng
		 * @Date: 2019-07-30 10:45:49
		 * @Desc: 打开预览dialog回调
		 */
		handlePreviewOpened(printData) {
			$('#preview_content').html(hiprintTemplate.getHtml(printData))
		},
		/**
		 * @Author: TangLiangcheng
		 * @Date: 2019-07-30 10:46:07
		 * @Desc: 打印
		 */
		printTemplate(printData) {
			hiprintTemplate.print(printData);
		},
		/**
		 * @Author: TangLiangcheng
		 * @Date: 2019-07-30 11:21:20
		 * @Desc: 设置element click事件监听
		 */
		elementAddEventListen() {
			let self = this
			window.hinnn.event.on(hiprintTemplate.getPrintElementSelectEventKey(), function (t) {
				self.paramsDrawerTitle = t.printElementType.title
				self.paramsDrawerStatus = true
			})
		},
		/**
		 * @Author: TangLiangcheng
		 * @Date: 2019-07-30 11:21:05
		 * @Desc: 参数设置drawer回调,将参数设置dom添加到弹出层中
		 */
		handleParamsOpened() {
			$('#params_content').append($('.hinnn-layout-sider'))
		},
		/**
		 * @Author: TangLiangcheng
		 * @Date: 2019-07-30 17:05:25
		 * @Desc: 获取配置JSON
		 */
		getJson() {
			this.infoTextarea = JSON.stringify(hiprintTemplate.getJson());
			this.infoDialogTitle = '预览JSON'
			this.infoDialogStatus = true;
		},
		/**
		 * @Author: TangLiangcheng
		 * @Date: 2019-07-30 17:05:07
		 * @Desc: 获取配置HTML
		 * @param printData
		 */
		getHtml(printData) {
			this.infoTextarea = hiprintTemplate.getHtml(printData)[0].outerHTML;
			this.infoDialogTitle = '预览HTML'
			this.infoDialogStatus = true;
		},
		/**
		 * @Author: TangLiangcheng
		 * @Date: 2019-07-31 09:45:15
		 * @Desc: 打开全屏
		 */
		openFullScreen() {
			$('.full_content_container').append($('.print_template_main'))
		},
		/**
		 * @Author: TangLiangcheng
		 * @Date: 2019-07-31 09:45:08
		 * @Desc: 关闭全屏
		 */
		closeFullScreen() {
			$('.print_template_container').append($('.print_template_main'))
		},
		/**
		 * @Author: TangLiangcheng
		 * @Date: 2019-07-31 09:44:53
		 * @Desc: 绑定数据源
		 */
		bindDataSources() {
			let eles = hiprintTemplate.getJson().panels[0].printElements
			if (eles) {
				this.dataSources = eles
			} else {
				this.dataSources = []
			}
			this.dataDialogStatus = true;
		}
	},
	mounted() {
		let self = this
		$(document).ready(function () {
			//初始化打印插件
			hiprint.init({
				providers: [new defaultElementTypeProvider()]
			});

			//设置左侧拖拽事件
			hiprint.PrintElementTypeManager.buildByHtml($('.ep-draggable-item'));
			let _customPrintJson = {};
			hiprintTemplate = new hiprint.PrintTemplate({
				template: _customPrintJson,
				settingContainer: '#PrintElementOptionSetting',
				paginationContainer: '.hiprint-printPagination'
			});
			//打印设计
			hiprintTemplate.design('#hiprint-printTemplate');
			self.setCurrentPaper(self.paperMap.A4);
			self.elementAddEventListen();
			$('.params_drawer').width($('.params_setting_container').width() * 1) //动态设置右部参数设置框宽度
		});
	}
}
</script>

<style>
.el-header,
.el-footer {
	background-color: #b3c0d1;
	color: #333;
	line-height: 60px;
}

.el-aside {
	background-color: #d3dce6;
	color: #333;
	padding: 6px;
}

.el-main {
	background-color: #e9eef3;
	color: #333;
	padding: 0;
}

.drag_item_box {
	height: 100%;
	padding: 6px;
}

.drag_item_box > div {
	height: 100%;
	width: 100%;
	background-color: #fff;
	display: flex;
	justify-content: center;
	align-items: center;
}

.drag_item_box > div > a {
	text-align: center;
	text-decoration-line: none;
}

.drag_item_box > div > a > span {
	font-size: 28px;
}

.drag_item_box > div > a > p {
	margin: 0;
}

.drag_item_title {
	font-size: 16px;
	padding: 12px 6px 0 6px;
	font-weight: bold;
}

.preview_dialog .el-dialog__body {
	padding: 0;
}

#params_content .container {
	width: 100% !important;
}

.params_drawer .el-drawer.rtl {
	overflow: auto;
}

.params_drawer .el-drawer__header {
	margin-bottom: 0;
}

.params_drawer {
	left: auto;
}

.hiprint-printTemplate {
	overflow-x: auto;
}

.hiprint-printPaper {
	background-color: #fff;
}

.full_screen_dialog .el-dialog__header {
	padding: 0;
	margin: 0;
}

.full_screen_dialog .el-dialog__body {
	padding: 0;
}
</style>
