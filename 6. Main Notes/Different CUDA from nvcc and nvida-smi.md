
```bash
nvcc --version
# maybe show different CUDA version
nvidia-smi
```

- **nvidia-smi**: Là công cụ quản lý và giám sát GPU ở mức driver, tương tác trực tiếp với phần cứng GPU thông qua driver NVIDIA. Nó cung cấp thông tin về trạng thái phần cứng (như sử dụng bộ nhớ, nhiệt độ) và phiên bản CUDA driver API, thuộc tầng thấp hơn (gần phần cứng hơn).
- **nvcc**: Là trình biên dịch của CUDA Toolkit, hoạt động ở mức cao hơn, thuộc tầng phát triển phần mềm. Nó biên dịch mã CUDA thành mã máy, phụ thuộc vào Toolkit và driver, nhưng không trực tiếp tương tác với phần cứng.

[link](https://stackoverflow.com/questions/53422407/different-cuda-versions-shown-by-nvcc-and-nvidia-smi)
![[ruMI3.png]]