### 多GPU
如果直接在可是写2，3没用
上面有几个写几个，下面再指定就可以了。
![[Pasted image 20240329000809.png|500]]

# 可视化
##### 提取前几张图片进行可视化  
def visualize_images(self,data_loader, num_images=5):  
    # 获取数据  
    images, labels = next(iter(data_loader))  
  
    # 将图片移到CPU并转换为numpy格式  
    images = images.cpu().numpy()  
  
    # 进行必要的反向变换，如果有进行标准化的话  
    # 假设使用的标准化是均值和标准差  
    # transform = transforms.Normalize(mean=[0.5, 0.5, 0.5], std=[0.5, 0.5, 0.5])  
  
    # 创建一个图形  
    fig, axes = plt.subplots(1, num_images, figsize=(15, 5))  
  
    for i in range(num_images):  
        # 获取单张图片  
        image = images[i]  
  
        # 转换为HWC格式并反转标准化（如果有的话）  
        image = (image.transpose(1, 2, 0) * 255).astype('uint8')  # 假设图片在[0, 1]范围  
  
        # 显示图片  
        axes[i].imshow(image)  
        axes[i].set_title(f'Label: {labels[i].item()}')  
        axes[i].axis('off')  
  
    plt.show()


### Trick

注入后门可以给目标客户端多跑几个本地轮次，宾夕法尼亚大学也干了