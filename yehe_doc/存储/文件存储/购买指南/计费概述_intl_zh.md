## 计费项

文件存储（Cloud File Storage，CFS）文件系统当前仅支持按存储量计费。文件系统创建时，会默认占用32MB的存储空间，该存储量将不会被计入实际使用的存储空间。

## 计费说明

CFS 的计费方式分为按量计费和资源包付费两种，说明如下：

<table>
   <tr>
      <th>计费项</th>
      <th>计费方式</th>
      <th>计费周期</th>
      <th>计费周期说明</th>
      <th>支持产品类型及地区</th>
   </tr>
   <tr>
      <td>存储量</td>
     <td><a href="https://cloud.tencent.com/document/product/582/47378">按量计费</a></td>
      <td>小时</td>
      <td>通用系列按每小时实际使用存储空间的最大值计费。<br>Turbo 系列按照购买容量计费，与实际使用量无关。</td>
      <td>提供服务的所有地域，具体请参见 <a href="https://intl.cloud.tencent.com/document/product/582/35772">可用地域</a>。</td>
   </tr>
     <tr>
      <td>存储量</td>
			<td><a href="https://cloud.tencent.com/document/product/582/47379">资源包</a></td>
      <td>包年包月</td>
      <td>在资源包有效期内，每小时进行后付费扣费时，资源包可以抵扣对应的文件系统存储用量。</td>
      <td>仅支持通用标准型/性能型，提供服务的所有地域，具体请参见 <a href="https://intl.cloud.tencent.com/document/product/582/35772">可用地域</a>。</td>
   </tr>
</table>





