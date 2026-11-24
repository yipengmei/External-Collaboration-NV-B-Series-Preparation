# 【外部合作】NV B系列准备

---

### 技术准备

基础环境：HGX 机器 node 内 8卡 nv-switch 互联。

#### 算子层

MegaMoE-fp8

- 难点1：目前业界方案只有 fwd，缺少 bwd 实现；
  - NV 在做 bwd 方案；MT侧预期9月初启动；
    - [ ] **NV 后续准备下相关方案，和MT做一个分享**
- 【低优】难点2：MegaMoE 不能跨 node；
  - nice to have 的 feature，希望27年有正式版本（可能有变数）。

DeepEP

- 难点1：DeepEPv2 在 “EP < 8，Hidden <= 1k” 场景下带宽利用率较低，100GB/s（理论 900GB nvlink）；
  - NOTE：外部 NIC 互联的是 b 单位。
  - [ ] **NV 后续准备下相关方案，和MT做一个分享**
- A：hybrid-EP，nccl-EP 备选方案。
  - [ ] **希望 NV 提供参考资料。**

使用 Blackwell 的 MxFp8 指令执行 DSv3 的 “blockwise[1, 128] + e8m0 scale” 的 fp8 量化方案

- Q：算子侧有什么 NV 可以提供的参考？
- A：参考 m-core 脚本，TE 实现；
  - [ ] **希望 NV 提供 kernel 级参考资料。**
  - [ ] **对比 MxFp8 vs Blockwise fp8 的性能&效果评估？**

单个 Node 内的 MoE 最佳实践？

- Q：对比过 NV hybrid-EP vs deepEP vs？
  - [ ] **希望提供一个对比参考。**

#### 框架层

MT侧准备尝试 Blockwise FP8 训练：

Q：NV 有 DSv4 flash 训练复现？TFlops？

- A：NV 复现业界方案参考：https://github.com/NVIDIA/Megatron-LM/tree/dev/examples/moe_recipes
- MT侧按照这个来预估吞吐。

**建议NV，按照 DSv3 + LongCat Flash（attn 可能是 longCat-Pro::LSA 结构） 的结构参考。**

### MT侧TODO

- [ ] 形成一个单卡目标吞吐？
- [ ] 形成一个外部 git-repo，用于交流。
