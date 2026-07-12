# xformers_blackwell_build
Xformers v0.0.32 build for Nvidia Blackwell (sm_120) and others arch (sm_100 - sm_75)

```xFormers 0.0.32+fde5a2f.d20260709``` built from source:

### Hardware tested

| Component | Value |
|---|---|
| CPU ARCH | amd64 / x86_64 |
| GPU | Nvidia RTX 5060 Ti (Blackwell, sm_120) |
| OS | Ubuntu 24.04 (Noble) |
| CUDA | 13.0 (V13.0.88) |
| Driver | 595.71.05 |
| cuDNN | 9.22.0 |
| Python | 3.12.13 |

### Build from source

I recommend install PyTorch and flash_attn first:
```
pip install torch==2.9.0 torchvision==0.24.0 torchaudio==2.9.0 --index-url https://download.pytorch.org/whl/cu130

pip install --no-cache-dir --no-build-isolation https://github.com/Dao-AILab/flash-attention/releases/download/v2.8.1/flash_attn-2.8.1+cu13torch2.9cxx11abiTRUE-cp312-cp312-linux_x86_64.whl
```

create ```nano constraints.txt``` (recent setuptools 80+ is missing ```pkg_resources```):
```
setuptools<75
wheel==0.41.3
```

Build command:
```bash
TORCH_CUDA_ARCH_LIST="7.5;8.0;8.6;8.9;9.0;10.0;12.0" \
MAX_JOBS=2 \
pip wheel --no-deps --no-build-isolation -c constraints.txt --wheel-dir=./dist -v git+https://github.com/facebookresearch/xformers.git@fde5a2fb46e3f83d73e2974a4d12caf526a4203e
```

### Verify after install

```python -m xformers.info``` after installed:
```
xFormers 0.0.32+fde5a2f.d20260709
memory_efficient_attention.ckF:                    unavailable
memory_efficient_attention.ckB:                    unavailable
memory_efficient_attention.ck_splitKF:             unavailable
memory_efficient_attention.cutlassF-pt:            available
memory_efficient_attention.cutlassB-pt:            available
memory_efficient_attention.fa2F@2.8.1:             available
memory_efficient_attention.fa2B@2.8.1:             available
memory_efficient_attention.fa3F@2.8.0.post2-3-g3ba6f82: available
memory_efficient_attention.fa3B@2.8.0.post2-3-g3ba6f82: available
memory_efficient_attention.fa3F_splitKV@2.8.0.post2-3-g3ba6f82: available
memory_efficient_attention.triton_splitKF:         available
indexing.scaled_index_addF:                        available
indexing.scaled_index_addB:                        available
indexing.index_select:                             available
sp24.sparse24_sparsify_both_ways:                  available
sp24.sparse24_apply:                               available
sp24.sparse24_apply_dense_output:                  available
sp24._sparse24_gemm:                               available
sp24._cslt_sparse_mm_search@0.8.0:                 available
sp24._cslt_sparse_mm@0.8.0:                        available
swiglu.dual_gemm_silu:                             available
swiglu.gemm_fused_operand_sum:                     available
swiglu.fused.p.cpp:                                available
is_triton_available:                               True
pytorch.version:                                   2.9.0+cu130
pytorch.cuda:                                      available
gpu.compute_capability:                            12.0
gpu.name:                                          NVIDIA GeForce RTX 5060 Ti
dcgm_profiler:                                     unavailable
build.info:                                        available
build.cuda_version:                                1300
build.hip_version:                                 None
build.python_version:                              3.12.13
build.torch_version:                               2.9.0+cu130
build.env.TORCH_CUDA_ARCH_LIST:                    7.5;8.0;8.6;8.9;9.0;10.0;12.0
build.env.PYTORCH_ROCM_ARCH:                       None
build.env.XFORMERS_BUILD_TYPE:                     None
build.env.XFORMERS_ENABLE_DEBUG_ASSERTIONS:        None
build.env.NVCC_FLAGS:                              None
build.env.XFORMERS_PACKAGE_FROM:                   None
build.nvcc_version:                                13.0.88
source.privacy:                                    open source
```


If flash_attn is not installed, the xformers fallback to its own flash_attn FA2 (v2.5.7):
```
xFormers 0.0.32+fde5a2f.d20260709
memory_efficient_attention.ckF:                    unavailable
memory_efficient_attention.ckB:                    unavailable
memory_efficient_attention.ck_splitKF:             unavailable
memory_efficient_attention.cutlassF-pt:            available
memory_efficient_attention.cutlassB-pt:            available
memory_efficient_attention.fa2F@2.5.7-pt:          available
memory_efficient_attention.fa2B@2.5.7-pt:          available
memory_efficient_attention.fa3F@2.8.0.post2-3-g3ba6f82: available
memory_efficient_attention.fa3B@2.8.0.post2-3-g3ba6f82: available
memory_efficient_attention.fa3F_splitKV@2.8.0.post2-3-g3ba6f82: available
memory_efficient_attention.triton_splitKF:         available
indexing.scaled_index_addF:                        available
indexing.scaled_index_addB:                        available
indexing.index_select:                             available
sp24.sparse24_sparsify_both_ways:                  available
sp24.sparse24_apply:                               available
sp24.sparse24_apply_dense_output:                  available
sp24._sparse24_gemm:                               available
sp24._cslt_sparse_mm_search@0.8.0:                 available
sp24._cslt_sparse_mm@0.8.0:                        available
swiglu.dual_gemm_silu:                             available
swiglu.gemm_fused_operand_sum:                     available
swiglu.fused.p.cpp:                                available
is_triton_available:                               True
pytorch.version:                                   2.9.0+cu130
pytorch.cuda:                                      available
gpu.compute_capability:                            12.0
gpu.name:                                          NVIDIA GeForce RTX 5060 Ti
dcgm_profiler:                                     unavailable
build.info:                                        available
build.cuda_version:                                1300
build.hip_version:                                 None
build.python_version:                              3.12.13
build.torch_version:                               2.9.0+cu130
build.env.TORCH_CUDA_ARCH_LIST:                    7.5;8.0;8.6;8.9;9.0;10.0;12.0
build.env.PYTORCH_ROCM_ARCH:                       None
build.env.XFORMERS_BUILD_TYPE:                     None
build.env.XFORMERS_ENABLE_DEBUG_ASSERTIONS:        None
build.env.NVCC_FLAGS:                              None
build.env.XFORMERS_PACKAGE_FROM:                   None
build.nvcc_version:                                13.0.88
source.privacy:                                    open source
```
