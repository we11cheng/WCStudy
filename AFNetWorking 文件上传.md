### AFNetWorking 文件上传（记录）

```
//
//  AddBeiAnViewController.m
//  CSSafeManager
//
//  Created by rby on 12/1/20.
//  Copyright © 2020 rby. All rights reserved.
//

#import "AddBeiAnViewController.h"

@interface AddBeiAnViewController () <UIDocumentPickerDelegate>
@property (weak, nonatomic) IBOutlet UITextView *nameTV;
@property (weak, nonatomic) IBOutlet UIButton *oneBtn;

@property (weak, nonatomic) IBOutlet UIButton *twoBtn;
@property (weak, nonatomic) IBOutlet UIButton *upBtn;
@property (weak, nonatomic) IBOutlet UIButton *saveBtn;
@property (nonatomic, copy) NSString *apiDocUrl;


@end

@implementation AddBeiAnViewController

- (IBAction)one:(id)sender {
    self.oneBtn.backgroundColor = kSaveBuleChoose;
    [self.oneBtn wcSetLayerWithCornerRadius:4.0 borderWidth:1.0 borderColor:kSaveBuleChoose];
    self.oneBtn.nbNormalTitleColor = [UIColor whiteColor];
    self.twoBtn.backgroundColor = [UIColor whiteColor];
    [self.twoBtn wcSetLayerWithCornerRadius:4.0 borderWidth:1.0 borderColor:kBorderCol];
    self.twoBtn.nbNormalTitleColor = [UIColor darkGrayColor];
}
- (IBAction)two:(id)sender {
    self.twoBtn.backgroundColor = kSaveBuleChoose;
    [self.twoBtn wcSetLayerWithCornerRadius:4.0 borderWidth:1.0 borderColor:kSaveBuleChoose];
    self.twoBtn.nbNormalTitleColor = [UIColor whiteColor];
    self.oneBtn.backgroundColor = [UIColor whiteColor];
    [self.oneBtn wcSetLayerWithCornerRadius:4.0 borderWidth:1.0 borderColor:kBorderCol];
    self.oneBtn.nbNormalTitleColor = [UIColor darkGrayColor];
}

- (IBAction)save:(id)sender {
    XLog(@"save");
}
- (IBAction)up:(id)sender {
    XLog(@"goSelcetPdf");
    [self goSelcetPdf];
}

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
                NSString *mineType = [self getMIMEType:pathUrl];
                [self showIndicatorView];
                [PPNetTool upLoadWithdata:@[pdfData].mutableCopy url:[NSString stringWithFormat:@"%@%@",YHSKkApiBaseUrlVersion,@"V1/teachingPlans/file"] filename:fileName name:@"file" params:@{} progress:^(NSProgress *progress) {
                    if (progress.completedUnitCount == progress.totalUnitCount) {
                        //[self hideIndicatorView];
                    } else {
                        [self showIndicatorView];
                    }
                } success:^(id responseObject) {
                    NSDictionary *resDic = (NSDictionary *)responseObject;
                    if ([resDic[@"status"] integerValue] == 1)  {
                        [self hideIndicatorView];
                        self.apiDocUrl = resDic[@"data"];
                    }
                    
                } failure:^(NSError *error) {
                    [self hideIndicatorView];
                       if (error) {
                           [self toastWithError:error];
                       }
                } failureBody:^(id  _Nonnull body) {
                    [self hideIndicatorView];
                      if (body) {
                          [self toastWithErrorMessage:body[kToastErrorMsg]];
                      }
                } mineType:mineType];
            }
            [self dismissViewControllerAnimated:YES completion:NULL];
        }];
        [urls.firstObject stopAccessingSecurityScopedResource];
    }else{
        //授权失败
    }
}

- (NSString*) getMIMEType: (NSString *) path
{
    if (![[NSFileManager defaultManager] fileExistsAtPath:path]) {
        return nil;
    }
    // Borrowed from http://stackoverflow.com/questions/5996797/determine-mime-type-of-nsdata-loaded-from-a-file
    // itself, derived from  http://stackoverflow.com/questions/2439020/wheres-the-iphone-mime-type-database
    CFStringRef UTI = UTTypeCreatePreferredIdentifierForTag(kUTTagClassFilenameExtension, (__bridge CFStringRef)[path pathExtension], NULL);
    CFStringRef mimeType = UTTypeCopyPreferredTagWithClass (UTI, kUTTagClassMIMEType);
    CFRelease(UTI);
    if (!mimeType) {
        return @"application/octet-stream";
    }
    return (__bridge NSString *)mimeType;
}

- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view from its nib.
    [self setNavTitle:@"添加备案"];
    self.nameTV.wc_placeholder = @"请输入";
    [self.upBtn wcSetLayerWithCornerRadius:20 borderWidth:1.0 borderColor:kSaveBA];
    [self.saveBtn wcSetLayerOnlyCornerRadius:20];
    [self.oneBtn wcSetLayerWithCornerRadius:4.0 borderWidth:1.0 borderColor:kSaveBuleChoose];
    [self.twoBtn wcSetLayerWithCornerRadius:4.0 borderWidth:1.0 borderColor:kBorderCol];
    self.apiDocUrl = @"";
}

/*
#pragma mark - Navigation

// In a storyboard-based application, you will often want to do a little preparation before navigation
- (void)prepareForSegue:(UIStoryboardSegue *)segue sender:(id)sender {
    // Get the new view controller using [segue destinationViewController].
    // Pass the selected object to the new view controller.
}
*/

@end
```

### 上传接口部门核心代码（搭配ppnetworker使用）
### .h文件
```
//
//  PPNetTool.h
//  CSSafeManager
//
//  Created by rby on 7/15/19.
//  Copyright © 2019 rby. All rights reserved.
//

#import <Foundation/Foundation.h>

NS_ASSUME_NONNULL_BEGIN

typedef NS_ENUM(NSUInteger, MethodType) {
	POST = 1,
	GET = 2,
	PATCH = 3,
	DELETE = 4,
	PUT = 5,
};

typedef void(^PPHttpRequestFailedBody)(id body);

@interface PPNetTool : NSObject

+ (void)requestWithUrlText:(NSString *)urlStr Method:(MethodType)type needCache:(BOOL)need parameterDic:(NSDictionary*)dic             responseCache: (PPHttpRequestCache)responseCache
success:(PPHttpRequestSuccess)success
failure:(PPHttpRequestFailed)failure
                   failureBody:(PPHttpRequestFailedBody)bodyBlock;

+ (void)requestWithUrl:(NSString *)urlStr Method:(MethodType)type needCache:(BOOL)need parameterDic:(NSDictionary*)dic 			responseCache: (PPHttpRequestCache)responseCache
			   	success:(PPHttpRequestSuccess)success
			   	failure:(PPHttpRequestFailed)failure
			   	failureBody:(PPHttpRequestFailedBody)bodyBlock;

//图片多图上传(表单提交)
+ (void)moreLoadWithImage:(NSMutableArray *)imagesArray
					  url:(NSString *)url
				 filename:(NSString *)filename
					 name:(NSString *)name
				   params:(NSDictionary *)params
				 progress:(PPHttpProgress)progress
				  success:(PPHttpRequestSuccess)success
				  failure:(PPHttpRequestFailed)failure
			  failureBody:(PPHttpRequestFailedBody)bodyBlock;
//多文件上传(表单提交)
+ (void)upLoadWithdata:(NSMutableArray *)dataArray
                      url:(NSString *)url
                 filename:(NSString *)filename
                     name:(NSString *)name
                   params:(NSDictionary *)params
                 progress:(PPHttpProgress)progress
                  success:(PPHttpRequestSuccess)success
                  failure:(PPHttpRequestFailed)failure
              failureBody:(PPHttpRequestFailedBody)bodyBlock mineType:(NSString *)mineType;
				 
@end

NS_ASSUME_NONNULL_END
```
#### .m

```
//
//  PPNetTool.m
//  CSSafeManager
//
//  Created by rby on 7/15/19.
//  Copyright © 2019 rby. All rights reserved.
//

#import "PPNetTool.h"
#import "NSError+YJPError.h"
#import "CommonErrorModel.h"

@interface PPNetTool ()

//@property (nonatomic, strong) AFHTTPSessionManager *sessionManager;

@end

@implementation PPNetTool

/*
- (AFHTTPSessionManager *)sessionManager {
	if (_sessionManager == nil) {
		_sessionManager = [AFHTTPSessionManager manager];
	}
	return _sessionManager;
}
 */

+ (NSString *)convert:(MethodType)type {
	switch (type) {
		case GET:
			return @"GET";
			break;
			case POST:
			return @"POST";
			break;
			case PUT:
			return @"PUT";
			break;
			case DELETE:
			return @"DELETE";
			break;
			case PATCH:
			return @"PATCH";
			break;
		default:
			break;
	}
}

+ (void)requestWithUrl:(NSString *)urlStr Method:(MethodType)type needCache:(BOOL)need parameterDic:(NSDictionary*)dic 			responseCache: (PPHttpRequestCache)responseCache
			   	success:(PPHttpRequestSuccess)success
			   	failure:(PPHttpRequestFailed)failure
				failureBody:(PPHttpRequestFailedBody)bodyBlock {
	XLog(@"=======%@=======%@=======%@=======",urlStr,dic,[self convert:type]);
	switch (type) {
		case GET: {
			if (need) {
				[PPNetworkHelper GET:urlStr parameters:dic responseCache:^(id responseCacheObject) {
					responseCache(responseCacheObject);
				} success:^(id responseObject) {
					success(responseObject);
                    if ([responseObject[kApistatus] integerValue] == 201) {
                        [[UIViewController wh_currentViewController] toastWithErrorMessage:responseObject[@"msg"]];
                    }
				} failure:^(NSError *error) {
					NSError *newError = [NSError returnErrorWithError:error];
					NSData *data = newError.userInfo[AFNetworkingOperationFailingURLResponseDataErrorKey];
					if (data) {
						id body = [NSJSONSerialization JSONObjectWithData:data options:NSJSONReadingMutableContainers error:nil];
						if (body&&[body isKindOfClass:[NSDictionary class]]) {
							//bodyBlock(body);
							NSDictionary *resDic = (NSDictionary *)body;
							CommonErrorModel *model = [[CommonErrorModel alloc] initWithDictionary:[NSDictionary changeType:resDic]];
							if (model.baseError.msgsAry.count ==0) {
								bodyBlock(@{kToastErrorMsg:model.msg});
							} else {
								if ([model.baseError.msgsAry[0] isKindOfClass:[NSString class]]) {
									bodyBlock(@{kToastErrorMsg:model.baseError.msgsAry[0]});
								} else if ([model.baseError.msgsAry[0] isKindOfClass:[NSArray class]]) {
									bodyBlock(@{kToastErrorMsg:model.baseError.msgsAry[0][0]});
								}
							}
						} else {
							failure(newError);
						}
					} else {
							//不是业务方面的错误
						failure(newError);
					}
				}];
			} else {
				//不需要缓存
				[PPNetworkHelper GET:urlStr parameters:dic success:^(id responseObject) {
					success(responseObject);
                    if ([responseObject[kApistatus] integerValue] == 201) {
                        [[UIViewController wh_currentViewController] toastWithErrorMessage:responseObject[@"msg"]];
                    }
				} failure:^(NSError *error) {
					NSError *newError = [NSError returnErrorWithError:error];
					NSData *data = newError.userInfo[AFNetworkingOperationFailingURLResponseDataErrorKey];
					if (data) {
						id body = [NSJSONSerialization JSONObjectWithData:data options:NSJSONReadingMutableContainers error:nil];
						if (body&&[body isKindOfClass:[NSDictionary class]]) {
							//bodyBlock(body);
							NSDictionary *resDic = (NSDictionary *)body;
							CommonErrorModel *model = [[CommonErrorModel alloc] initWithDictionary:[NSDictionary changeType:resDic]];
							if (model.baseError.msgsAry.count ==0) {
								bodyBlock(@{kToastErrorMsg:model.msg});
							} else {
								if ([model.baseError.msgsAry[0] isKindOfClass:[NSString class]]) {
									bodyBlock(@{kToastErrorMsg:model.baseError.msgsAry[0]});
								} else if ([model.baseError.msgsAry[0] isKindOfClass:[NSArray class]]) {
									bodyBlock(@{kToastErrorMsg:model.baseError.msgsAry[0][0]});
								}
							}
						} else {
							failure(newError);
						}
					} else {
							//不是业务方面的错误
						failure(newError);
					}
				}];
			};
		}
			break;
		case POST: {
			if (need) {
				[PPNetworkHelper POST:urlStr parameters:dic responseCache:^(id responseCacheObject) {
					responseCache(responseCacheObject);
				} success:^(id responseObject) {
					success(responseObject);
                    if ([responseObject[kApistatus] integerValue] == 201) {
                        [[UIViewController wh_currentViewController] toastWithErrorMessage:responseObject[@"msg"]];
                    }
				} failure:^(NSError *error) {
					NSError *newError = [NSError returnErrorWithError:error];
					NSData *data = newError.userInfo[AFNetworkingOperationFailingURLResponseDataErrorKey];
					if (data) {
						id body = [NSJSONSerialization JSONObjectWithData:data options:NSJSONReadingMutableContainers error:nil];
						if (body&&[body isKindOfClass:[NSDictionary class]]) {
							//bodyBlock(body);
							NSDictionary *resDic = (NSDictionary *)body;
							CommonErrorModel *model = [[CommonErrorModel alloc] initWithDictionary:[NSDictionary changeType:resDic]];
							if (model.baseError.msgsAry.count ==0) {
								bodyBlock(@{kToastErrorMsg:model.msg});
							} else {
								if ([model.baseError.msgsAry[0] isKindOfClass:[NSString class]]) {
									bodyBlock(@{kToastErrorMsg:model.baseError.msgsAry[0]});
								} else if ([model.baseError.msgsAry[0] isKindOfClass:[NSArray class]]) {
									bodyBlock(@{kToastErrorMsg:model.baseError.msgsAry[0][0]});
								}
							}
						} else {
							failure(newError);
						}
					} else {
							//不是业务方面的错误
						failure(newError);
					}
				}];
			} else {
				[PPNetworkHelper POST:urlStr parameters:dic success:^(id responseObject) {
					success(responseObject);
                    if ([responseObject[kApistatus] integerValue] == 201) {
                        [[UIViewController wh_currentViewController] toastWithErrorMessage:responseObject[@"msg"]];
                    }
				} failure:^(NSError *error) {
					NSError *newError = [NSError returnErrorWithError:error];
					NSData *data = newError.userInfo[AFNetworkingOperationFailingURLResponseDataErrorKey];
					if (data) {
						id body = [NSJSONSerialization JSONObjectWithData:data options:NSJSONReadingMutableContainers error:nil];
						if (body&&[body isKindOfClass:[NSDictionary class]]) {
							//bodyBlock(body);
							NSDictionary *resDic = (NSDictionary *)body;
							CommonErrorModel *model = [[CommonErrorModel alloc] initWithDictionary:[NSDictionary changeType:resDic]];
							
							if (model.baseError.msgsAry.count ==0) {
								bodyBlock(@{kToastErrorMsg:model.msg});
							} else {
								if ([model.baseError.msgsAry[0] isKindOfClass:[NSString class]]) {
									bodyBlock(@{kToastErrorMsg:model.baseError.msgsAry[0]});
								} else if ([model.baseError.msgsAry[0] isKindOfClass:[NSArray class]]) {
									bodyBlock(@{kToastErrorMsg:model.baseError.msgsAry[0][0]});
								}
							}
						} else {
							failure(newError);
						}
					} else {
							//不是业务方面的错误
						failure(newError);
					}
				}];
			}
		}
			break;
		case PATCH: {
			AFHTTPSessionManager *manager = [AFHTTPSessionManager manager];
			[manager PATCH:urlStr parameters:dic success:^(NSURLSessionDataTask * _Nonnull task, id  _Nullable responseObject) {
				success(responseObject);
                if ([responseObject[kApistatus] integerValue] == 201) {
                    [[UIViewController wh_currentViewController] toastWithErrorMessage:responseObject[@"msg"]];
                }
			} failure:^(NSURLSessionDataTask * _Nullable task, NSError * _Nonnull error) {
				NSError *newError = [NSError returnErrorWithError:error];
				NSData *data = newError.userInfo[AFNetworkingOperationFailingURLResponseDataErrorKey];
				if (data) {
					id body = [NSJSONSerialization JSONObjectWithData:data options:NSJSONReadingMutableContainers error:nil];
					if (body&&[body isKindOfClass:[NSDictionary class]]) {
						//bodyBlock(body);
						NSDictionary *resDic = (NSDictionary *)body;
						CommonErrorModel *model = [[CommonErrorModel alloc] initWithDictionary:[NSDictionary changeType:resDic]];
						if (model.baseError.msgsAry.count ==0) {
							bodyBlock(@{kToastErrorMsg:model.msg});
						} else {
							if ([model.baseError.msgsAry[0] isKindOfClass:[NSString class]]) {
								bodyBlock(@{kToastErrorMsg:model.baseError.msgsAry[0]});
							} else if ([model.baseError.msgsAry[0] isKindOfClass:[NSArray class]]) {
								bodyBlock(@{kToastErrorMsg:model.baseError.msgsAry[0][0]});
							}
						}
					} else {
						failure(newError);
					}
				} else {
						//不是业务方面的错误
					failure(newError);
				}
			}];
		}
			break;
		case PUT: {
			[PPNetworkHelper PUT:urlStr parameters:dic responseCache:^(id responseCache) {
				
			} success:^(id responseObject) {
				success(responseObject);
                if ([responseObject[kApistatus] integerValue] == 201) {
                    [[UIViewController wh_currentViewController] toastWithErrorMessage:responseObject[@"msg"]];
                }
			} failure:^(NSError *error) {
				NSError *newError = [NSError returnErrorWithError:error];
				NSData *data = newError.userInfo[AFNetworkingOperationFailingURLResponseDataErrorKey];
				if (data) {
					id body = [NSJSONSerialization JSONObjectWithData:data options:NSJSONReadingMutableContainers error:nil];
					if (body&&[body isKindOfClass:[NSDictionary class]]) {
						//bodyBlock(body);
						NSDictionary *resDic = (NSDictionary *)body;
						CommonErrorModel *model = [[CommonErrorModel alloc] initWithDictionary:[NSDictionary changeType:resDic]];
						if (model.baseError.msgsAry.count ==0) {
							bodyBlock(@{kToastErrorMsg:model.msg});
						} else {
							if ([model.baseError.msgsAry[0] isKindOfClass:[NSString class]]) {
								bodyBlock(@{kToastErrorMsg:model.baseError.msgsAry[0]});
							} else if ([model.baseError.msgsAry[0] isKindOfClass:[NSDictionary class]]) {
							NSDictionary *resDic =model.baseError.msgsAry[0];
							NSArray *arr = [resDic allValues];
							bodyBlock(@{kToastErrorMsg:arr[0]});
							}
						}
					} else {
						failure(newError);
					}
				} else {
						//不是业务方面的错误
					failure(newError);
				}
			}];
		}
			break;
		case DELETE: {
			[PPNetworkHelper DEL:urlStr parameters:dic responseCache:^(id responseCache) {
				
			} success:^(id responseObject) {
				success(responseObject);
                if ([responseObject[kApistatus] integerValue] == 201) {
                    [[UIViewController wh_currentViewController] toastWithErrorMessage:responseObject[@"msg"]];
                }
			} failure:^(NSError *error) {
				NSError *newError = [NSError returnErrorWithError:error];
				NSData *data = newError.userInfo[AFNetworkingOperationFailingURLResponseDataErrorKey];
				if (data) {
					id body = [NSJSONSerialization JSONObjectWithData:data options:NSJSONReadingMutableContainers error:nil];
					if (body&&[body isKindOfClass:[NSDictionary class]]) {
						//bodyBlock(body);
						NSDictionary *resDic = (NSDictionary *)body;
						CommonErrorModel *model = [[CommonErrorModel alloc] initWithDictionary:[NSDictionary changeType:resDic]];
						if (model.baseError.msgsAry.count ==0) {
							bodyBlock(@{kToastErrorMsg:model.msg});
						} else {
							if ([model.baseError.msgsAry[0] isKindOfClass:[NSString class]]) {
								bodyBlock(@{kToastErrorMsg:model.baseError.msgsAry[0]});
							} else if ([model.baseError.msgsAry[0] isKindOfClass:[NSArray class]]) {
								bodyBlock(@{kToastErrorMsg:model.baseError.msgsAry[0][0]});
							}
						}
					} else {
						failure(newError);
					}
				} else {
						//不是业务方面的错误
					failure(newError);
				}
			}];
		}
			break;
			
		default:
			break;
	}
}

+ (void) moreLoadWithImage:(NSMutableArray *)imagesArray
					  url:(NSString *)url
				 filename:(NSString *)filename
					 name:(NSString *)name
				   params:(NSDictionary *)params
				 progress:(PPHttpProgress)progress
				  success:(PPHttpRequestSuccess)success
				  failure:(PPHttpRequestFailed)failure
			  failureBody:(PPHttpRequestFailedBody)bodyBlock {
	AFHTTPSessionManager *manager = [AFHTTPSessionManager manager];
	[manager.requestSerializer setValue:@"application/x-www-form-urlencoded" forHTTPHeaderField:@"Content-Type"];
	manager.requestSerializer = [AFHTTPRequestSerializer serializer];
	manager.responseSerializer = [AFJSONResponseSerializer serializer];
	// . 设置响应数据类型
	manager.responseSerializer.acceptableContentTypes = [NSSet setWithObjects:@"text/plain", @"multipart/form-data", @"application/json", @"text/html", @"image/jpeg", @"image/png", @"application/octet-stream", @"text/json", nil];
	url = [url stringByAddingPercentEncodingWithAllowedCharacters:[NSCharacterSet URLQueryAllowedCharacterSet]];
	[manager POST:url parameters:params constructingBodyWithBlock:^(id<AFMultipartFormData>  _Nonnull formData) {
		for (NSInteger i = 0; i < imagesArray.count; i ++) {
				//压缩图片
			NSData *imageData = UIImageJPEGRepresentation(imagesArray[i], 0.5);
			NSString *imageFileName =filename;
			if (filename == nil || [filename isKindOfClass:[NSString class]] || filename.length == 0) {
				NSDateFormatter *formatter = [[NSDateFormatter alloc]init];
				formatter.dateFormat = @"yyyy-MM-dd-HH-mm-ss";
				NSString *str = [formatter stringFromDate:[NSDate date]];
				imageFileName = [NSString stringWithFormat:@"%@.jpg",str];//以这种格式防止上传的图片重复覆盖
			}
				//上传图片，以文件流的格式
			[formData appendPartWithFileData:imageData name:name fileName:imageFileName mimeType:@"image/jpeg"];
			
		}
	
	} progress:^(NSProgress * _Nonnull uploadProgress) {
		XLog(@"上传速度--%lld,总进度--%lld",uploadProgress.completedUnitCount,uploadProgress.totalUnitCount);
		if (progress) {
			progress(uploadProgress);
		}
	} success:^(NSURLSessionDataTask * _Nonnull task, id  _Nullable responseObject) {
		
		NSLog(@"上传图片成功-%@",responseObject);
		if (success) {
			success(responseObject);
		}
		
		
	} failure:^(NSURLSessionDataTask * _Nullable task, NSError * _Nonnull error) {
		XLog(@"error=%@",error);
		NSError *newError = [NSError returnErrorWithError:error];
		NSData *data = newError.userInfo[AFNetworkingOperationFailingURLResponseDataErrorKey];
		if (data) {
			id body = [NSJSONSerialization JSONObjectWithData:data options:NSJSONReadingMutableContainers error:nil];
			if (body&&[body isKindOfClass:[NSDictionary class]]) {
				//bodyBlock(body);
				NSDictionary *resDic = (NSDictionary *)body;
				CommonErrorModel *model = [[CommonErrorModel alloc] initWithDictionary:[NSDictionary changeType:resDic]];
				if (model.baseError.msgsAry.count ==0) {
					bodyBlock(@{kToastErrorMsg:model.msg});
				} else {
					bodyBlock(@{kToastErrorMsg:model.baseError.msgsAry[0]});
				}
			} else {
				failure(newError);
			}
		} else {
				//不是业务方面的错误
			failure(newError);
		}
	}];
	
}

//name字段后台提供//filename本地路径名
+ (void)upLoadWithdata:(NSMutableArray *)dataArray
        url:(NSString *)url
   filename:(NSString *)filename
       name:(NSString *)name
     params:(NSDictionary *)params
   progress:(PPHttpProgress)progress
    success:(PPHttpRequestSuccess)success
    failure:(PPHttpRequestFailed)failure
           failureBody:(PPHttpRequestFailedBody)bodyBlock mineType:(NSString *)mineType{
    AFHTTPSessionManager *manager = [AFHTTPSessionManager manager];
    [manager.requestSerializer setValue:@"application/x-www-form-urlencoded" forHTTPHeaderField:@"Content-Type"];
    manager.requestSerializer = [AFHTTPRequestSerializer serializer];
    manager.responseSerializer = [AFJSONResponseSerializer serializer];
    // . 设置响应数据类型
    manager.responseSerializer.acceptableContentTypes = [NSSet setWithObjects:@"text/plain", @"multipart/form-data", @"application/json", @"text/html", @"image/jpeg", @"image/png", @"application/octet-stream", @"text/json", nil];
    url = [url stringByAddingPercentEncodingWithAllowedCharacters:[NSCharacterSet URLQueryAllowedCharacterSet]];
    [manager POST:url parameters:params constructingBodyWithBlock:^(id<AFMultipartFormData>  _Nonnull formData) {
        for (NSInteger i = 0; i < dataArray.count; i ++) {
                //压缩图片
            NSData *imageData = dataArray[i];
            
                //上传图片，以文件流的格式
            //[formData appendPartWithFileData:imageData name:name fileName:filename mimeType:@"multipart/form-data"];
            [formData appendPartWithFileData:imageData name:name fileName:filename mimeType:mineType];
            
        }
    
    } progress:^(NSProgress * _Nonnull uploadProgress) {
        XLog(@"上传速度--%lld,总进度--%lld",uploadProgress.completedUnitCount,uploadProgress.totalUnitCount);
        if (progress) {
            progress(uploadProgress);
        }
    } success:^(NSURLSessionDataTask * _Nonnull task, id  _Nullable responseObject) {
        
        NSLog(@"上传图片成功-%@",responseObject);
        if (success) {
            success(responseObject);
        }
        
        
    } failure:^(NSURLSessionDataTask * _Nullable task, NSError * _Nonnull error) {
        XLog(@"error=%@",error);
        NSError *newError = [NSError returnErrorWithError:error];
        NSData *data = newError.userInfo[AFNetworkingOperationFailingURLResponseDataErrorKey];
        if (data) {
            id body = [NSJSONSerialization JSONObjectWithData:data options:NSJSONReadingMutableContainers error:nil];
            if (body&&[body isKindOfClass:[NSDictionary class]]) {
                //bodyBlock(body);
                NSDictionary *resDic = (NSDictionary *)body;
                CommonErrorModel *model = [[CommonErrorModel alloc] initWithDictionary:[NSDictionary changeType:resDic]];
                if (model.baseError.msgsAry.count ==0) {
                    bodyBlock(@{kToastErrorMsg:model.msg});
                } else {
                    bodyBlock(@{kToastErrorMsg:model.baseError.msgsAry[0]});
                }
            } else {
                failure(newError);
            }
        } else {
                //不是业务方面的错误
            failure(newError);
        }
    }];
}

+ (void)requestWithUrlText:(NSString *)urlStr Method:(MethodType)type needCache:(BOOL)need parameterDic:(NSDictionary*)dic             responseCache: (PPHttpRequestCache)responseCache
success:(PPHttpRequestSuccess)success
failure:(PPHttpRequestFailed)failure
                   failureBody:(PPHttpRequestFailedBody)bodyBlock {
    XLog(@"=======%@=======%@=======%@=======",urlStr,dic,[self convert:type]);
    switch (type) {
        case POST: {
            [[self sessionManager] POST:urlStr parameters:dic progress:^(NSProgress * _Nonnull uploadProgress) {
                
            } success:^(NSURLSessionDataTask * _Nonnull task, id  _Nullable responseObject) {
                success(responseObject);
                NSDictionary *resDictem = [NSJSONSerialization JSONObjectWithData:responseObject options:NSJSONReadingMutableContainers error:nil];
                XLog(@"TextHtmlApi返回------%@",resDictem);
            } failure:^(NSURLSessionDataTask * _Nullable task, NSError * _Nonnull error) {
                NSError *newError = [NSError returnErrorWithError:error];
                NSData *data = newError.userInfo[AFNetworkingOperationFailingURLResponseDataErrorKey];
                if (data) {
                    id body = [NSJSONSerialization JSONObjectWithData:data options:NSJSONReadingMutableContainers error:nil];
                    if (body&&[body isKindOfClass:[NSDictionary class]]) {
                        //bodyBlock(body);
                        NSDictionary *resDic = (NSDictionary *)body;
                        CommonErrorModel *model = [[CommonErrorModel alloc] initWithDictionary:[NSDictionary changeType:resDic]];
                        if (model.baseError.msgsAry.count ==0) {
                            bodyBlock(@{kToastErrorMsg:model.msg});
                        } else {
                            if ([model.baseError.msgsAry[0] isKindOfClass:[NSString class]]) {
                                bodyBlock(@{kToastErrorMsg:model.baseError.msgsAry[0]});
                            } else if ([model.baseError.msgsAry[0] isKindOfClass:[NSArray class]]) {
                                bodyBlock(@{kToastErrorMsg:model.baseError.msgsAry[0][0]});
                            }
                        }
                    } else {
                        failure(newError);
                    }
                } else {
                        //不是业务方面的错误
                    failure(newError);
                }
            }];
        }
            break;
            
        default:
            break;
    }
}

+ (AFHTTPSessionManager *)sessionManager{
	AFHTTPSessionManager *sessionManager = [AFHTTPSessionManager manager];
	sessionManager.requestSerializer.timeoutInterval = 60.f;
    sessionManager.responseSerializer = [AFHTTPResponseSerializer serializer];
    sessionManager.requestSerializer = [AFJSONRequestSerializer serializer];
	sessionManager.responseSerializer.acceptableContentTypes = [NSSet setWithObjects:@"application/json", @"text/html", @"text/json", @"text/plain", @"text/javascript", @"text/xml", @"image/*", nil];
	return sessionManager;
}

@end


```