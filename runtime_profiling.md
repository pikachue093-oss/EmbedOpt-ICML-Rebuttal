# Runtime and GPU Memory Profiling

## Setup

- **GPU**: NVIDIA H100 PCIe (80 GB)
- **Diffusion steps**: 100
- **N_sample**: 1 (single trajectory)
- **LR**: 0.1 (EmbedOpt and DPS)
- **Map resolution**: 5A
- **Seeds**: 101, 42, 16 (3 seeds per method)
- **Timing**: measured inside the sampler's diffusion loop (`torch.cuda.synchronize()` + `time.time()`), excludes model loading, MSA processing, and trunk forward pass (shared across all methods)
- **Peak GPU memory**: `torch.cuda.max_memory_allocated()` over the diffusion loop

## Results

### 8P4K_A (599 residues)

| Method | Time (s) | Peak GPU (GB) | Time vs Prior | Memory vs Prior |
|--------|----------|---------------|---------------|-----------------|
| Prior | 9.0 ± 0.3 | 5.22 | 1.0x | 1.0x |
| DPS (Guided) | 23.0 ± 0.2 | 5.82 | 2.6x | 1.1x |
| EmbedOpt | 37.2 ± 0.5 | 7.48 | 4.1x | 1.4x |

### 8K23_B (177 residues)

| Method | Time (s) | Peak GPU (GB) | Time vs Prior | Memory vs Prior |
|--------|----------|---------------|---------------|-----------------|
| Prior | 4.2 ± 0.3 | 2.09 | 1.0x | 1.0x |
| DPS (Guided) | 17.2 ± 0.1 | 1.96 | 4.1x | 0.9x |
| EmbedOpt | 25.3 ± 0.1 | 2.09 | 6.1x | 1.0x |
