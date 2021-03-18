### iOS UIDocumentPickerViewController使用 以及右上角按钮显示
#### 1.首先是在info.plist文件配置一下（增加两个key）：

```
Supports opening documents in place

Application supports iTunes file sharing
```
#### 设置为true

#### 方法调用

```
#pragma mark -- 选择文件
- (void)goSelcetPdf  {
    NSArray *documentTypes = @[@"com.microsoft.powerpoint.​ppt",
    @"com.microsoft.word.doc",
    @"com.microsoft.excel.xls",
    @"com.microsoft.powerpoint.​pptx",
    @"com.microsoft.word.docx",
    @"com.microsoft.excel.xlsx",
    @"public.avi",
    @"public.3gpp",
    @"public.mpeg-4",
    @"com.compuserve.gif",
    @"public.jpeg",
    @"public.png",
    @"public.plain-text",
    @"com.adobe.pdf"];
    UIDocumentPickerViewController *documentPickerViewController = [[UIDocumentPickerViewController alloc] initWithDocumentTypes:documentTypes inMode:UIDocumentPickerModeOpen];
    [[UINavigationBar appearance] setTranslucent:NO];
    documentPickerViewController.delegate = self;
    [self presentViewController:documentPickerViewController animated:YES completion:nil];
}

#pragma mark - UIDocumentPickerDelegate
- (void)documentPicker:(UIDocumentPickerViewController *)controller didPickDocumentsAtURLs:(nonnull NSArray<NSURL *> *)urls {
    BOOL fileUrlAuthozied = [urls.firstObject startAccessingSecurityScopedResource];
    if(fileUrlAuthozied) {
        //通过文件协调工具来得到新的文件地址，以此得到文件保护功能
        NSFileCoordinator *fileCoordinator = [[NSFileCoordinator alloc] init];
        NSError*error;
        __block NSString *fileName;
        __block NSData *pdfData;
        __block NSString *pathUrl;
      [fileCoordinator coordinateReadingItemAtURL:urls.firstObject options:0 error:&error byAccessor:^(NSURL *newURL) {
            //读取文件
            fileName = [newURL lastPathComponent];
            NSError*error =nil;
            NSData *fileData = [NSData dataWithContentsOfURL:newURL options:NSDataReadingMappedIfSafe error:&error];

            if(error) {

                //读取出错
            }else{
                //保存
                pdfData= fileData;
               NSArray*paths  =NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES);
                NSString*path = [paths objectAtIndex:0];
                path = [path stringByAppendingString:@"/YWBG"];
                if (![[NSFileManager defaultManager] fileExistsAtPath:path]) {
                    [[NSFileManager defaultManager] createDirectoryAtPath:path withIntermediateDirectories:YES attributes:nil error:nil];
                }
                pathUrl = [path stringByAppendingPathComponent:fileName];
                [pdfData writeToFile:pathUrl atomically:YES];
            }
            [self dismissViewControllerAnimated:YES completion:NULL];
        }];
        [urls.firstObject stopAccessingSecurityScopedResource];
    }else{
        //授权失败
    }
}
```
#### 注意的地方，`[[UINavigationBar appearance] setTranslucent:NO];`可以解决UIImagePickerController iOS13调起相册 中的照片被导航栏遮挡的问题，记录一下
