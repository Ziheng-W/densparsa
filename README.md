A first-author paper accepted by DAC 2025.

Aside from the rigorous discussion in the paper, my personal opinions on it is as follows. If I'm a reviewer, I would write those stuff:

**Summary**

A light-weight method to let systolic arrays accelerate unstructured sparse matrix, regardless of single-side or double-side sparsity. 

**Pros**

1. At least for the papers I've read, past designs of systolic arrays support only double-side sparsity, thus limited within CNN acceleration. This paper provides a solution for single-side sparsity, making the design applicable for LLM.
   
   I think the way we enable the design with single-side sparsity is somehow like a brainteaser. It's simple but difficult to come up with.

2. A bonus point for this design is that most additional parts for sparsity acceleration is decoupled from the baseline design. Thus, those parts can be gated when executing dense workloads, leading to a reduced power overhead.

3. The overhead (for both area and power) is small. Again, the design is simple. Thus, the efficiency is still acceptable if we use this sparsity accelerator to do dense operations.

**Cons**

1. The speedup ratio is not good for single-side high sparsity, where one matrix input is dense and another one has less than 1% non-zeros. In such case, the speedup ratio does't scale up as the matrix density scales down. (But for other cases it scales well.)

2. The work is concentrated on the core array of a matrix engine, with the assumption that compressed matrix inputs can be smoothly fetched. So it's not a system-level solution. I guess that for highly sparse matrix the compressed format and memory-array interface would require additional much optimization, but that's not discussed in this paper.

**Additional Personal Ideas**

1. I would say this design is already close to the optimal one among the systolic-array-based sparsity accelerators. The hardware overhead is already low, and if we want higher speedup we have to turn to other baseline architecture (maybe like reduction tree?). A very good example for supporting mutiple dataflow is **Trapezoid**, a design from MIT. It sacrifices area overhead to achieve surprising speedup ratio.

2. A big problem for nearly all sparsity accelerators I've seen is that they are limited for a given data width, since the dataflow is designed under a certain data width. Thus a design for FP32 cannot be reused for FP16. On the other hand, supporting multiple data type is a highly valued merit for any real-life application.

3. Supporting unstructured sparsity is not rewarding for small data type like FP8. The multiplier and adder are smaller and the overhead becomes larger. 

4. All designs I've seen achieves a roughly linear acceleration. If both input matrix has 0.1 density, the speedup ratio is at the magnitude of 10. Of course we want it to be at magnitude of 100.

**P.S.**

I'm just a senior undergraduate, please forgive me if I'm talking trash.
