# RapidVideoSDK
.NET API Library for rapidvideo.com

`Download:`[https://github.com/loudKode/RapidVideoSDK/releases](https://github.com/loudKode/RapidVideoSDK/releases)
# Functions:
```vb.net
    Function UserInfo() As Task(Of JSON_UserInfo)
    Function List() As Task(Of JSON_ListFolder)
    Function Upload(FileToUpload As Object, UploadType As VClient.UploadTypes, UserID As String, FileName As String, Optional ReportCls As IProgress(Of ReportStatus) = Nothing, Optional _proxi As ProxyConfig = Nothing, Optional token As Threading.CancellationToken = Nothing) As Task(Of JSON_Upload)
    Function RemoteUpload(FileUrl As String) As Task(Of JSON_RemoteUpload)
    Function GetSplash(DestinationFile As String) As Task(Of JSON_GetSplash)
    Function GenerateDownloadUrl(DestinationFileID As String, UrlVaildFor As VClient.VaildForEnum) As Task(Of JSON_GenerateDownloadUrl)
    Function FileInfo(DestinationFileID As String) As Task(Of JSON_FileInfo)
    Function RunningConvertsQueue(Optional DestinationFolder As String = Nothing) As Task(Of JSON_RunningConvertsQueue)
    Function RemoteUploadStatus(JobID As String) As Task(Of JSON_RemoteUploadStatus)
```

# Example:
```vb.net
Dim cLENT As RapidVideoSDK.IClient = New RapidVideoSDK.VClient("xxxxxxxxxxx", "xxxxxxxxxxx")
Dim RSLT = Await cLENT.UserInfo
Dim RSLT = Await cLENT.List()
Dim RSLT = Await cLENT.RemoteUpload("https://sample-videos.com/video123/mp4/720/big_buck_bunny_720p_5mb.mp4")
Dim RSLT = Await cLENT.RemoteUploadStatus("46218")
Dim RSLT = Await cLENT.GetSplash("G5DDS7S3KA")
Dim RSLT = Await cLENT.FileInfo("G1K1EEYQ2K")
        Try
            Dim frm As New DeQma.FileFolderDialogs.VistaOpenFileDialog With {.Multiselect = False, .Title = "Select image/s to upload"}
            If frm.ShowDialog = DialogResult.OK AndAlso Not String.IsNullOrEmpty(frm.FileName) Then
                Dim UploadCancellationToken As New Threading.CancellationTokenSource()
                Dim progressIndicator_ReportCls As New Progress(Of RapidVideoSDK.ReportStatus)(Sub(ReportClass As RapidVideoSDK.ReportStatus)
                                                                                                   Label1.Text = String.Format("{0}/{1}", ISisFunctions.Bytes_To_KbMbGb.SetBytes(ReportClass.BytesTransferred), ISisFunctions.Bytes_To_KbMbGb.SetBytes(ReportClass.TotalBytes))
                                                                                                   ProgressBar1.Value = CInt(ReportClass.ProgressPercentage)
                                                                                                   Label2.Text = If(CStr(ReportClass.TextStatus) Is Nothing, "Uploading...", CStr(ReportClass.TextStatus))
                                                                                               End Sub)
                Await cLENT.Upload(frm.FileName, RapidVideoSDK.VClient.UploadTypes.FilePath, "229457", IO.Path.GetFileName(frm.FileName), progressIndicator_ReportCls, Nothing, UploadCancellationToken.Token)
            End If
        Catch ex As RapidVideoSDK.VeryStreamException
            MsgBox(ex.Message)
        End Try
```
