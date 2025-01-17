## 2021年04月

<table>
	<thead>
		<tr>
			<th width="20%">动态名称</th>
			<th width="50%">动态描述</th>
			<th width="15%">发布时间</th>
			<th width="15%">相关文档</th>
		</tr>
	</thead>
	<tbody>
		<tr><td>LogListener 服务日志上线</td><td>LogListener 服务日志功能支持记录 LogListener 端运行状态和采集监控的日志数据并配置可视化视图，提供重要指标数据。</td><td>2021-04-26</td><td><a href="https://cloud.tencent.com/document/product/614/55281">LogListener 服务日志</a></td></tr>
	</tbody>
</table>

## 2021年03月

<table>
	<thead>
		<tr>
			<th width="20%">动态名称</th>
			<th width="50%">动态描述</th>
			<th width="15%">发布时间</th>
			<th width="15%">相关文档</th>
		</tr>
	</thead>
	<tbody>
		<tr><td>支持上传解析失败日志</td><td>所有解析失败的日志，均以 LogParseFailure 作为键名称（Key），原始日志内容作为值（Value）进行上传。</td><td>2021-03-26</td><td>-</td></tr>
		<tr><td>莫斯科、泰国新区上线</td><td>新增欧洲莫斯科、亚太曼谷地域，日志服务部署上线。</td><td>2021-03-20</td><td><a href="https://intl.cloud.tencent.com/document/product/614/18940">可用地域</a></td></tr>
	</tbody>
</table>

## 2021年02月

<table>
	<thead>
		<tr>
			<th width="20%">动态名称</th>
			<th width="50%">动态描述</th>
			<th width="15%">发布时间</th>
			<th width="15%">相关文档</th>
		</tr>
	</thead>
	<tbody>
		<tr><td>LogListener 自动升级</td><td>支持用户在控制台预设时间段指定机器组进行 agent 自动升级，也可对目标机器实行手动升级。</td><td>2021-02-27</td><td><a href="https://cloud.tencent.com/document/product/614/55468">LogListener 升级指南</a></td></tr>
		<tr><td>CLS 全面支持毫秒级精度日志</td><td>LogListener 使用采集时间支持毫秒级时间戳。开启时间采集后，LogListener 携带毫秒级的 Unix 时间戳进行上传。</td><td>2021-02-20</td><td>-</td></tr>
		<tr><td>百万级日志下载</td><td>最高可支持百万级日志下载，支持指定检索条件、检索时间范围自定义导出所需的日志数量，提供【CSV】和【JSON】两种导出格式。</td><td>2021-02-14</td><td><a href="https://intl.cloud.tencent.com/document/product/614/34234">日志下载</a></td></tr>
		<tr><td>上下文检索</td><td><ul  style="margin: 0;"><li>增加定位当前日志功能，快速定位目标日志滚动查看上下文。</li><li>增加多个字符串高亮功能，快速标记出用户检索的关键词。</li><li>增加过滤条件功能，快速定位目标字符串所在日志。</li></ul></td><td>2021-02-06</td><td><a href="https://intl.cloud.tencent.com/document/product/614/39795">上下文检索分析</a></td></tr>
	</tbody>
</table>

## 2020年12月

<table>
	<thead>
		<tr>
			<th width="20%">动态名称</th>
			<th width="50%">动态描述</th>
			<th width="15%">发布时间</th>
			<th width="15%">相关文档</th>
		</tr>
	</thead>
	<tbody>
		<tr><td>监控告警2.0发布</td><td>CLS 支持对日志主题设置告警策略，查询分析结果满足触发条件时用户可及时接收告警通知，实时监控日志数据。</td><td>2020-12-15</td><td><a href="https://intl.cloud.tencent.com/document/product/614/39573">监控告警</a></td></tr>
		<tr><td>CLS 支持对接 Grafana</td><td>CLS 与 Grafana 打通，支持将 CLS 的原始日志数据与 SQL 聚合分析结果导出至 Grafana 展示。前往 <a href="http://106.53.153.13:3000/d/r6mrhEbGz/cls-demo.com">Grafana_demo</a> 立即体验。</td><td>2020-12-15</td><td><a href="https://intl.cloud.tencent.com/document/product/614/39592">CLS 对接 Grafana</a></td></tr>
		<tr><td>多行日志支持正则提取</td><td>LogListener 采集配置规则新增【多行-完全正则】提取模式采集日志。</td><td>2020-12-15</td><td><a href="https://intl.cloud.tencent.com/document/product/614/39590">多行-完全正则</a></td></tr>
		<tr><td>检索页2.0版本上新</td><td><ul  style="margin: 0;"><li>时间组件支持毫秒级，快速输入【自定义时间】，提高查询效率。</li><li>日志数据展示新增【原始布局和表格布局】功能以及【是否开启换行】功能，方便灵活切换。</li><li>新增偏好设置，用户可【自定义日志加载数量】、【历史记录数量】，满足数据浏览所需。</li></ul></td><td>2020-12-15</td><td>-</td></tr>
	</tbody>
</table>

## 2020年11月

<table>
	<thead>
		<tr>
			<th width="20%">动态名称</th>
			<th width="50%">动态描述</th>
			<th width="15%">发布时间</th>
			<th width="15%">相关文档</th>
		</tr>
	</thead>
	<tbody>
		<tr><td>SQL 统计分析全量开放</td><td>CLS 提供 SQL 统计分析能力，用户可有对日志数据进行聚合统计，并支持以图表的形式展示分析结果。</td><td>2020-11-24</td><td><a href="https://intl.cloud.tencent.com/document/product/614/37803">分析简介</a></td></tr>
		<tr><td>投递云函数</td><td>CLS 支持将日志主题中的数据通过 CLS 的日志触发器投递至云函数，以满足日志数据 ETL 场景。</td><td>2020-11-20</td><td><a href="https://intl.cloud.tencent.com/document/product/614/38883">函数处理简介</a></td></tr>
		<tr><td>CLS 助力容器服务（事件&审计中心）</td><td>CLS 与 TKE 联合推出集群审计与事件日志中心，用户可通过可视化图表实时查看审计日志和集群事件，轻松提升容器集群运维效率。</td><td>2020-11-03</td><td><a href="https://intl.cloud.tencent.com/document/product/457/38338">集群审计</a></td></tr>
	</tbody>
</table>

## 2020年10月

<table>
	<thead>
		<tr>
			<th width="20%">动态名称</th>
			<th width="50%">动态描述</th>
			<th width="15%">发布时间</th>
			<th width="15%">相关文档</th>
		</tr>
	</thead>
	<tbody>
		<tr><td>主题分区自动分裂</td><td>开启自动分裂功能后，如果主题分区持续触达了写请求或者写流量的阈值，日志服务会根据实际写入情况，自动分裂至合理的分区数。</td><td>2020-10-25</td><td><a href="https://intl.cloud.tencent.com/document/product/614/39587">主题分区</a></td></tr>
		<tr><td>字段快速分析2.0</td><td>字段快速分析重构改版：<ul  style="margin: 0;"><li>用户可更方便快捷查看字段统计结果。</li><li>支持快速更改显示字段并保存。</li><li>通过拖拽字段快速更改字段显示顺序。</li></ul></td><td>2020-10-15</td><td>-</td></tr>
	</tbody>
</table>

## 2020年09月

<table>
	<thead>
		<tr>
			<th width="20%">动态名称</th>
			<th width="50%">动态描述</th>
			<th width="15%">发布时间</th>
			<th width="15%">相关文档</th>
		</tr>
	</thead>
	<tbody>
		<tr><td>台北、首尔新区上线</td><td>新增台北、首尔新地域，日志服务部署上线。</td><td>2020-09-27</td><td><a href="https://intl.cloud.tencent.com/document/product/614/18940">可用地域</a></td></tr>
		<tr><td>检索页支持版面配置</td><td>检索页提供 LogListener 采集配置与索引配置入口，用户可快速查看机器组状态，索引字段信息。</td><td>2020-09-24</td><td>-</td></tr>
		<tr><td>Lucene 语法全面支持</td><td>CLS 全面支持 Lucene 语法检索。</td><td>2020-09-18</td><td><a href="https://intl.cloud.tencent.com/document/product/614/30439">语法与规则</a></td></tr>
		<tr><td>免费额度发布</td><td>CLS 商业化后，腾讯云仍旧为所有用户在每个地域提供一定量的免费额度。</td><td>2020-09-12</td><td><a href="https://intl.cloud.tencent.com/document/product/614/37889">免费额度</a></td></tr>
	</tbody>
</table>

