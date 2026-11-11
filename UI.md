# 统一问诊工作台（2026-11-10）

首页从三个纵向实验面板改为同一患者/会话下的左右工作台。

- 顶部患者代号与新会话是共同上下文；切换患者会重置录音、文字、身份结果和检索依据。录音/分析或检索处理中禁止切换，避免串用上下文。
- 左侧：录音控制、已确认脱敏文字、“身份与回听”标签。停止、清空、同意选项保留；断线测试与传输计数收进连接详情。
- 右侧：当前患者病历与相关来源。默认自动使用左侧已确认转写（最新 2000 字），手动输入后可切回对话。检索仍须点击，不自动发请求；内容更新后提示旧结果需重新检索。
- 病历导入/列表/TXT 输入位于右侧“管理历史病历”；测试问诊须明确点击填入，初始页面不填假对话。
- 脱敏自动处理；完整隐私说明和本地测试收进“隐私与数据”原生模态框。服务链路检查和独立 WAV 精校页入口位于“工作区设置”。没有删除后端能力。
- 新会话保留同一患者已导入的历史，清空当次问诊与检索结果；服务器原文保留规则不变。刷新页面仍创建新工作区，需重新导入病历。
- 桌面双栏分别滚动，录音按钮留在对话面板底部；窄屏 CSS 改为上下排列，收窄导航。未执行手机设备实测。

验证：多页构建、19 项既有前端测试通过。浏览器验证病历导入/检索、身份标签、隐私模态框、新会话清空与保留病历、患者切换。未发起云端录音或付费 API 调用，未重新验收语音质量。


## Action guidance update

The workspace remains freely navigable; no wizard or mandatory step sequence was added. The patient bar now suggests a next action from actual recording, history/query and result state. A first-time user sees Import 3 sample records directly in Relevant history. After import, it becomes a records-ready state. Custom TXT input and record management remain secondary. Recording explains cloud consent and optional audio retention beside the control. Search explains missing records or query text beside the disabled button. The sample-query action supports a free text-only test. Browser checks cover sample query before import, direct import, enabled search and changing next-action advice. No microphone or paid cloud recording is started by these checks.


## Resizable panes

Conversation and Relevant history can be resized vertically using their native bottom-right handles. Each pane has an independent height. Speaker results no longer have a nested 380px maximum; the enclosing pane supplies scrolling. Conversations with results default to a taller 680px minimum initial size, while manual resizing can reduce height to 440px. Resized height lasts in the current DOM; page refresh or a remount resets it. CSS-only hot update preserves existing page audio and transcript state. Browser drag verified Conversation from 460px to 486px while History stayed 460px; build passed.


History search uses the confirmed conversation by default, without repeating its full transcript in an editable box. View search context reveals a read-only preview when confirmed text exists. Custom search reveals a separate search phrase; Test tools contains the sample query. Returning to the conversation clears previous custom results. Import controls and cited source cards are unchanged.
