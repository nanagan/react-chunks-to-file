# React-chunks-to-file
用 React 实现的大文件切片下载库，支持多并发下载。

## 如何使用
```ts
import { ChunksDownload } from 'react-chunks-to-file';
```

## 参数说明
```ts
/**
 *
 * @param reqSetting
 * 相关接口地址与参数(getSizeAPI: 用于获取文件的大小，确定发送请求的个数；getSizeParams: 获取文件大小接口的参数；chunkDownloadAPI: 分片下载；chunkDownloadParams: 分片下载接口的参数)
 *   1. 先请求文件大小接口（使用 axios 发送请求）
 *   getSizeAPI:
 *     method: GET
 *     request params: getSizeParams
 *     response example: {
 *         status: 200,
 *         data: {
 *             size: number
 *         }
 *     }
 *    2. 随后根据文件大小决定请求多少次获取文件切片的接口
 *    chunkDownloadAPI:
 *      method: GET
 *      request params: chunkDownloadParams
 *      response example: (需要返回文件相应片段的[]byte)
 *    
 *    注：前端默认通过请求 header 中的 Range 字段来决定请求文件的什么片段，也可以自定义参数
 *    
 * @param fileName
 * 需要保存成什么名字的文件
 * @param mime
 * 文件的类型
 * @param size
 * 分片大小（单位兆-M）
 * @param concurrency
 * 下载文件的并发线程数
 * @param setStatus (非必选）
 * 设置当前状态（1：获取文件大小出现错误；2: 下载切片出现错误；）
 * @param setPercent (非必选）
 * 文件上传进度（0-100）
 *
 */
```

## 示例代码
```ts
  // 进度
  const [percent, setPercent] = useState<number>();
  // 状态
  const [status, setStatus] = useState<number>();
  
  const requestSetting = {
      getSizeAPI: "",
      getSizeParams: {},
      chunkDownloadAPI: "",
      chunkDownloadParams: {},
  }
  return (
    <div>
      <ChunksDownload 
        reqSetting={requestSetting}
        fileName="test.csv"
        mime="text/csv"
        size={3}
        concurrency={3}
        setPercent={setPercent}
        setStatus={setStatus}
      >
        <Button type='primary'>点击下载</Button>
      </ChunksUpload>
  )
```