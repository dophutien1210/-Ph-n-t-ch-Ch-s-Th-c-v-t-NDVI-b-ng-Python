# 🌍 Phan tich chi so thuc vat (NDVI) bằng Python

Dự án này minh họa quy trình đọc dữ liệu ảnh vệ tinh, tính toán chỉ số NDVI (Normalized Difference Vegetation Index) và xuất kết quả trực quan hóa bằng Python.

## 🛠️ Công nghệ & Thư viện
* `rasterio`: Đọc và ghi dữ liệu raster.
* `numpy`: Xử lý mảng và tính toán toán học.
* `matplotlib`: Trực quan hóa dữ liệu không gian.

## 🚀 Hướng dẫn cài đặt và sử dụng
1. Clone repository này về máy: `git clone [link-repo-cua-ban]`
2. Cài đặt môi trường: `pip install -r requirements.txt`
3. Đặt file dữ liệu Landsat (Band 4 và Band 5) vào thư mục `data/`.
4. Chạy script: `python src/calculate_ndvi.py`

## 📊 Kết quả minh họa
![Bản đồ NDVI](outputs/ndvi_sample_map.png)
*(Ghi chú: Thay thế bằng ảnh chụp màn hình bản đồ đầu ra thực tế của bạn)*

import rasterio
import numpy as np
import matplotlib.pyplot as plt

def calculate_ndvi(red_path, nir_path):
    """
    Đọc các band ảnh vệ tinh và tính toán ma trận NDVI.
    
    Input:
        red_path (str): Đường dẫn đến file Red Band (.tif).
        nir_path (str): Đường dẫn đến file Near Infrared Band (.tif).
    Output:
        Mảng numpy 2D chứa giá trị NDVI.
    """
    with rasterio.open(red_path) as src_red:
        red = src_red.read(1).astype('float32')
        
    with rasterio.open(nir_path) as src_nir:
        nir = src_nir.read(1).astype('float32')

    # Bỏ qua cảnh báo chia cho 0
    np.seterr(divide='ignore', invalid='ignore')
    
    # Tính toán NDVI
    ndvi = np.where(
        (nir + red) == 0., 
        0, 
        (nir - red) / (nir + red)
    )
    return ndvi

def visualize_ndvi(ndvi_matrix, output_path):
    """Trực quan hóa và lưu bản đồ NDVI."""
    plt.figure(figsize=(10, 8))
    plt.imshow(ndvi_matrix, cmap='RdYlGn')
    plt.colorbar(label='NDVI')
    plt.title('Bản đồ Chỉ số Thực vật NDVI')
    plt.savefig(output_path)
    print(f"Đã lưu bản đồ tại: {output_path}")

if __name__ == "__main__":
    # Đường dẫn giả định
    RED_BAND = "../data/landsat_band4.tif"
    NIR_BAND = "../data/landsat_band5.tif"
    OUTPUT_MAP = "../outputs/ndvi_sample_map.png"
    
    # Thực thi
    # ndvi_result = calculate_ndvi(RED_BAND, NIR_BAND)
    # visualize_ndvi(ndvi_result, OUTPUT_MAP)
