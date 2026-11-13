# Performance Evaluation and Optimization in Parallel Computing
**Student ID:** 230103316
**Report date:** 2026-11-13
**Hardware:** Apple M2, macOS

## Phase 0 — Hardware Topology and Telemetry

| Parameter | Value |
|---|---|
| CPU | Apple M2 |
| Physical Cores | 8 |
| Logical Threads | 8 |
| Cache Line | 128 bytes |
| L1 Data Cache | 64 KB (reported) |
| L2 Cache | 4 MB (reported) |
| L3 Cache | Not reported by sysctl |

### Timer Calibration

| Metric | Value |
|---|---:|
| Calls | 1,000,000 |
| Minimum | 40.7632 ms |
| Median | 44.3573 ms |
| IQR | 3.6260 ms |
| Average latency | 44.3573 ns/call |

The measurements were obtained locally using time.perf_counter_ns().

## Verification Signatures

```text
VERIFY_HASH[PHASE0]: 925FFF4170EBB83D (P_log=8)
VERIFY_HASH[PHASE1]: C524A1E7CD24F096 (P_log=8)
VERIFY_HASH[PHASE2]: AE5650D86C424461 (P_log=8)
VERIFY_HASH[PHASE3]: 72D7E947CB4EB92A (P_log=8)
VERIFY_HASH[PHASE4]: A6BF54033E234781 (P_log=8)
```

## Phase 1: False Sharing

| p | stride | offset_bytes | min_ms | median_ms | iqr_ms | throughput_mops | speedup | efficiency | karp_flatt |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 8 | 1 | 8 | 479.716458 | 512.805166 | 54.559875000000034 | 31.200933728503042 | 4.0086529022993505 | 0.5010816127874188 | 0.14224041253399708 |
| 8 | 2 | 16 | 489.912584 | 505.582041 | 15.056873999999993 | 31.646693716322094 |  |  |  |
| 8 | 4 | 32 | 514.017542 | 519.24125 | 14.004790999999955 | 30.814192824626318 |  |  |  |
| 8 | 8 | 64 | 512.774334 | 530.780583 | 29.249625000000037 | 30.14428280244758 |  |  |  |
| 8 | 16 | 128 | 504.915958 | 527.935 | 14.50799900000004 | 30.3067612490174 | 3.9725108204608524 | 0.49656385255760654 | 0.1448342428958504 |
| 8 | 32 | 256 | 528.023125 | 543.701459 | 23.717708000000016 | 29.427914409918845 |  |  |  |
| 1 | 1 | 8 | 2054.35225 | 2055.657917 | 96.58970899999986 | 7.783396190427534 | 1.0 | 1.0 | 0.0 |
| 2 | 1 | 8 | 1104.923542 | 1162.498792 | 55.788749999999936 | 13.763455162368889 | 1.768309723112383 | 0.8841548615561915 | 0.1310235836286764 |
| 4 | 1 | 8 | 656.149417 | 677.987792 | 24.755166999999915 | 23.599244984635355 | 3.0319984242430134 | 0.7579996060607533 | 0.10642063311743133 |
| 1 | 16 | 128 | 2037.911667 | 2097.2275 | 87.47545800000012 | 7.629119873738066 | 1.0 | 1.0 | 0.0 |
| 2 | 16 | 128 | 1072.4765 | 1087.184041 | 38.04845799999998 | 14.716919487967356 | 1.929045516590691 | 0.9645227582953455 | 0.036782171700495025 |
| 4 | 16 | 128 | 627.542209 | 650.017541 | 41.914792000000034 | 24.614720358754134 | 3.226416777574314 | 0.8066041943935786 | 0.07992181169345407 |

## Phase 2: Strong Scaling

| p | N | time | stage_a | checksum | speedup | efficiency |
| --- | --- | --- | --- | --- | --- | --- |
| 1 | 1200000 | 1.1241082920023473 | 0.023328249997575767 | 749347.2186864461 | 1.0 | 1.0 |
| 2 | 1200000 | 0.5802835000067716 | 0.02346483299334068 | 749347.218686085 | 1.9371708690480247 | 0.9685854345240124 |
| 3 | 1200000 | 0.43634208299044985 | 0.023175375012215227 | 749347.2186860188 | 2.5762087495625545 | 0.8587362498541848 |
| 4 | 1200000 | 0.31711133300268557 | 0.023327457995037548 | 749347.218686087 | 3.5448379638731717 | 0.8862094909682929 |
| 5 | 1200000 | 0.3018124579975847 | 0.023453000001609325 | 749347.2186861355 | 3.724525817987749 | 0.7449051635975498 |
| 6 | 1200000 | 0.27909433300374076 | 0.02373395799077116 | 749347.2186861543 | 4.027700168269918 | 0.6712833613783197 |
| 7 | 1200000 | 0.2623530000128085 | 0.024168250005459413 | 749347.218686173 | 4.284716744033674 | 0.6121023920048106 |
| 8 | 1200000 | 0.2526977500092471 | 0.024834666997776367 | 749347.2186861921 | 4.448430158009765 | 0.5560537697512207 |

## Phase 2: Weak Scaling

| p | N | time | stage_a | checksum | weak_efficiency | alpha | gustafson |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | 200000 | 0.18507712500286289 | 0.0038120000099297613 | 124891.20311435821 | 1.0 | 0.020596818811999564 | 1.0 |
| 2 | 400000 | 0.19604041700949892 | 0.007790583011228591 | 249782.40622871617 | 0.9440763686699114 | 0.039739677817820125 | 1.96026032218218 |
| 3 | 600000 | 0.20724004200019408 | 0.01184958399971947 | 374673.6093430745 | 0.8930567819644118 | 0.0571780621416221 | 2.885643875716756 |
| 4 | 800000 | 0.2154408750066068 | 0.016332166007487103 | 499564.81245743396 | 0.8590622601081955 | 0.07580811211886442 | 3.7725756636434067 |
| 5 | 1000000 | 0.26174420800816733 | 0.02036654099356383 | 624456.0155717934 | 0.7070915777325923 | 0.07781085644091243 | 4.688756574236351 |
| 6 | 1200000 | 0.29243525001220405 | 0.024847291002515703 | 749347.2186861543 | 0.6328824072855074 | 0.08496681231650002 | 5.5751659384175 |
| 7 | 1400000 | 0.3132076670008246 | 0.029301208996912464 | 874238.4218005158 | 0.5909086670039135 | 0.0935520170291212 | 6.438687897825273 |
| 8 | 1600000 | 0.3446355829946697 | 0.03425749999587424 | 999129.6249148773 | 0.5370226817401075 | 0.09940209800217897 | 7.304185313984747 |

## Phase 3: Roofline

| metric | value |
| --- | --- |
| bandwidth_GBs | 26.51704973160996 |
| compute_GFLOPs | 0.8758582014498661 |
| ridge_FLOP_per_byte | 0.033030001840883116 |

## Phase 3: GEMM

| variant | tile_size | median_seconds | gflops | tile_bytes | intensity | roofline_gflops | fits_L1 | fits_L2 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | 0 | 0.23345887499453966 | 0.14372737811224695 | 0 | 0 |  |  |  |
| 2 | 0 | 0.25427200000558514 | 0.13196274854983234 | 0 | 0 |  |  |  |
| 3 | 8 | 0.29953508300241083 | 0.11202170932254348 | 1536 | 0.6666666666666666 | 0.8758582014498661 | True | True |
| 3 | 16 | 0.26510216601309367 | 0.12657170065650356 | 6144 | 1.3333333333333333 | 0.8758582014498661 | True | True |
| 3 | 32 | 0.2563487500010524 | 0.1308936829216536 | 24576 | 2.6666666666666665 | 0.8758582014498661 | True | True |
| 3 | 64 | 0.2549028750072466 | 0.13163614572431198 | 98304 | 5.333333333333333 | 0.8758582014498661 | False | True |
| 3 | 96 | 0.2681776660028845 | 0.12512015821496145 | 221184 | 8.0 | 0.8758582014498661 | False | True |
| 3 | 128 | 0.2583851250092266 | 0.12986208861211268 | 393216 | 10.666666666666666 | 0.8758582014498661 | False | True |

## Phase 4: Scheduling

| strategy | chunk | wall_seconds | imbalance_lambda | min_worker_seconds | max_worker_seconds | checksum |
| --- | --- | --- | --- | --- | --- | --- |
| Block Static | 0 | 3.591183791999356 | 2.4462529949808793 | 0.018914000000222586 | 3.485053208001773 | 9029.98456392221 |
| Cyclic Static | 0 | 2.1125781669979915 | 1.0261319998934022 | 1.929234124996583 | 2.018962708010804 | 9029.98456392221 |
| Dynamic | 1 | 2.153693125001155 | 1.0034730877743308 | 2.052370042001712 | 2.0672182499984046 | 9029.984563922197 |
| Dynamic | 8 | 2.1923812500026543 | 1.0077885894707512 | 2.0791754169913474 | 2.111912084001233 | 9029.984563922191 |
| Dynamic | 64 | 2.3056937500077765 | 1.0644947500372322 | 1.9499467919958988 | 2.2014535000053 | 9029.984563922191 |
| Dynamic | 512 | 2.957729083005688 | 1.4633905061812613 | 0.9793934169865679 | 2.8696587910089875 | 9029.984563922157 |

## Diagnostic Analysis

### Phase 1 — False Sharing

The stride sweep showed no clear reduction in cache
contention at 128 bytes. At 8 workers, stride 1 took
512.81 ms and stride 16 took 527.93 ms. Increasing
process count improved speedup but reduced efficiency.

### Phase 2 — Strong and Weak Scaling

Strong scaling achieved a speedup of 4.4484 at
8 processes, with efficiency of 55.61%.
The measured serial fraction was 0.020753,
giving an Amdahl limit of 48.1866.

Weak scaling efficiency at 8 processes was 53.70%.
Gustafson's scaled speedup reached 7.3042.

The results show that parallel execution improves
performance, but scheduling overhead and serial
work limit scaling on the Apple M2.

### Phase 3 — Roofline and GEMM

Measured memory bandwidth was 26.5170 GB/s.
The Python compute ceiling was 0.8759 GFLOP/s.
The empirical ridge point was 0.0330 FLOP/byte.

The best blocked GEMM used BS=64 and achieved
0.13164 GFLOP/s. Naive GEMM achieved approximately
0.14373 GFLOP/s.

All GEMM accuracy checks passed.

Based on the reported cache sizes:

- BS=8, 16, and 32 fit within the reported L1 size.
- BS=64, 96, and 128 exceed the L1 size.
- All tested tile footprints fit within the reported L2 size.

All blocked GEMM configurations achieved less
performance than the estimated compute roofline.
This is consistent with Python interpreter overhead
and other implementation costs.

### Phase 4 — Dynamic Load Balancing

Block Static required 3.5912 seconds and had
a load imbalance factor of 2.4463.

Cyclic Static was the fastest overall scheduling
strategy at 2.1126 seconds.

The best dynamic chunk was C=1, requiring
2.1537 seconds and achieving 1.6675x speedup
over Block Static.

Small chunks distribute uneven workloads more
evenly, but require frequent synchronization.
Larger chunks reduce synchronization overhead
while increasing the risk of tail imbalance.

### Implementation Notes

All timing values were measured locally on Apple M2.
The compute roofline represents the measured Python
kernel rather than the hardware's theoretical peak.

The Phase 2 workload uses ten sin/sqrt calls in
Stage A and ten repeated arithmetic/trigonometric
expressions in Stage B. Its exact operation counting
convention should be considered when interpreting
the results.

## Complete Python Source Files

### harness.py

```python
from dataclasses import dataclass
import multiprocessing as mp
import time
from typing import Callable


@dataclass
class BenchStats:
    min_ms: float
    median_ms: float
    iqr_ms: float


def fnv1a_hash(s: str) -> int:
    h = 14695981039346656037
    for b in s.encode("utf-8"):
        h = ((h ^ b) * 1099511628211) & 0xFFFFFFFFFFFFFFFF
    return h


def run_benchmark(
    fn: Callable[[], None],
    warmup: int = 2,
    runs: int = 7
) -> BenchStats:
    for _ in range(warmup):
        fn()

    samples = []
    for _ in range(runs):
        t0 = time.perf_counter_ns()
        fn()
        samples.append(
            (time.perf_counter_ns() - t0) / 1e6
        )

    samples.sort()
    med = samples[runs // 2]

    return BenchStats(
        samples[0],
        med,
        samples[(3 * runs) // 4] - samples[runs // 4]
    )


def emit_verify_tag(
    phase: str,
    seed: int,
    m1: float,
    m2: float
) -> str:
    hw = mp.cpu_count()
    sig = fnv1a_hash(
        f"{phase}|{seed}|{hw}|{m1:.4f}|{m2:.4f}"
    )
    return (
        f"VERIFY_HASH[{phase}]: {sig:016X} "
        f"(P_log={hw})"
    )

```

### task0_cal.py

```python
import multiprocessing as mp
import time

from harness import run_benchmark, emit_verify_tag

STUDENT_ID = 230103316
CALLS = 1_000_000


def measure_timer():
    for _ in range(CALLS):
        time.perf_counter_ns()


def main():
    logical_cores = mp.cpu_count()

    stats = run_benchmark(
        measure_timer,
        warmup=2,
        runs=7
    )

    avg_latency_ns = (
        stats.median_ms * 1_000_000 / CALLS
    )

    print("=== PHASE 0 ===")
    print(f"CPU: Apple M2")
    print(f"Logical cores: {logical_cores}")
    print(f"Timer calls: {CALLS:,}")
    print(f"Minimum: {stats.min_ms:.4f} ms")
    print(f"Median: {stats.median_ms:.4f} ms")
    print(f"IQR: {stats.iqr_ms:.4f} ms")
    print(
        f"Average call latency: "
        f"{avg_latency_ns:.4f} ns"
    )

    print(
        emit_verify_tag(
            "PHASE0",
            STUDENT_ID,
            avg_latency_ns,
            stats.median_ms
        )
    )


if __name__ == "__main__":
    main()

```

### task1_falsesharing.py

```python
import csv
import ctypes
import multiprocessing as mp
from multiprocessing import shared_memory

from harness import run_benchmark, emit_verify_tag

STUDENT_ID = 230103316
M = 16_000_000
SHM_SIZE = 4096
STRIDES = [1, 2, 4, 8, 16, 32]


def fs_worker(shm_name, slot_idx, iters):
    shm = shared_memory.SharedMemory(name=shm_name)
    arr = (ctypes.c_uint64 * 512).from_buffer(shm.buf)

    try:
        acc = arr[slot_idx]
        for i in range(iters):
            acc = (
                acc * 6364136223846793005 + (i | 1)
            ) & 0xFFFFFFFFFFFFFFFF
            arr[slot_idx] = acc
    finally:
        del arr
        shm.close()


def run_trial(p, stride):
    shm = shared_memory.SharedMemory(
        create=True,
        size=SHM_SIZE
    )

    try:
        shm.buf[:SHM_SIZE] = b'\x00' * SHM_SIZE

        base, extra = divmod(M, p)
        processes = []

        for worker_id in range(p):
            iters = base + (worker_id < extra)

            proc = mp.Process(
                target=fs_worker,
                args=(
                    shm.name,
                    worker_id * stride,
                    iters
                )
            )
            processes.append(proc)

        for proc in processes:
            proc.start()

        for proc in processes:
            proc.join()

        for proc in processes:
            if proc.exitcode != 0:
                raise RuntimeError(
                    f"Worker failed: {proc.exitcode}"
                )

    finally:
        shm.close()
        shm.unlink()


def benchmark_case(p, stride):
    stats = run_benchmark(
        lambda: run_trial(p, stride),
        warmup=1,
        runs=7
    )

    return {
        "p": p,
        "stride": stride,
        "offset_bytes": stride * 8,
        "min_ms": stats.min_ms,
        "median_ms": stats.median_ms,
        "iqr_ms": stats.iqr_ms,
        "throughput_mops": M / stats.median_ms / 1000
    }


def main():
    logical = mp.cpu_count()
    physical = 8

    print("=== PHASE 1 ===", flush=True)
    print(f"Logical cores: {logical}", flush=True)
    print(f"Physical cores: {physical}", flush=True)
    print(f"Total operations: {M:,}", flush=True)

    rows = []
    results = {}

    print("\n--- STRIDE SWEEP ---", flush=True)

    for stride in STRIDES:
        r = benchmark_case(physical, stride)
        rows.append(r)
        results[(physical, stride)] = r

        print(
            f"stride={stride:2d} | "
            f"offset={stride * 8:3d} bytes | "
            f"median={r['median_ms']:.2f} ms | "
            f"IQR={r['iqr_ms']:.2f} ms | "
            f"throughput={r['throughput_mops']:.2f} Mops/s",
            flush=True
        )

    print("\n--- PROCESS SCALING ---", flush=True)

    process_counts = [1, 2, 4, 8]

    for stride in [1, 16]:
        for p in process_counts:
            if (p, stride) in results:
                r = results[(p, stride)]
            else:
                r = benchmark_case(p, stride)
                rows.append(r)
                results[(p, stride)] = r

            baseline = results.get((1, stride))

            if baseline is None:
                baseline = r

            speedup = (
                baseline["median_ms"] / r["median_ms"]
            )
            efficiency = speedup / p

            if p == 1:
                karp_flatt = 0.0
            else:
                karp_flatt = (
                    1 / speedup - 1 / p
                ) / (1 - 1 / p)

            r["speedup"] = speedup
            r["efficiency"] = efficiency
            r["karp_flatt"] = karp_flatt

            print(
                f"stride={stride:2d} | "
                f"p={p} | "
                f"T={r['median_ms']:.2f} ms | "
                f"S={speedup:.4f} | "
                f"E={efficiency:.4f} | "
                f"e={karp_flatt:.4f}",
                flush=True
            )

    with open(
        "phase1_results.csv",
        "w",
        newline=""
    ) as f:
        fields = [
            "p", "stride", "offset_bytes",
            "min_ms", "median_ms", "iqr_ms",
            "throughput_mops", "speedup",
            "efficiency", "karp_flatt"
        ]

        writer = csv.DictWriter(
            f,
            fieldnames=fields
        )
        writer.writeheader()
        writer.writerows(rows)

    m1 = results[(physical, 1)]["median_ms"]
    m2 = results[(physical, 16)]["median_ms"]

    print("\n--- VERIFICATION ---", flush=True)
    print(
        emit_verify_tag(
            "PHASE1",
            STUDENT_ID,
            m1,
            m2
        ),
        flush=True
    )

    print(
        "\nResults saved to phase1_results.csv",
        flush=True
    )


if __name__ == "__main__":
    mp.freeze_support()
    main()

```

### task2_scaling.py

```python
import csv
import math
import multiprocessing as mp
import time
from statistics import median

from harness import emit_verify_tag

STUDENT_ID = 230103316
P_LOG = mp.cpu_count()
P_PHYS = 8

N_STRONG = 1_200_000
N0_WEAK = 200_000
RUNS = 3


def stage_a(n):
    total = 0.0
    count = int(n * 0.05)

    for i in range(count):
        x = (i % 1000 + 1) * 0.001
        for _ in range(5):
            x = math.sin(x) + math.sqrt(abs(x) + 1.0)
        total += x

    return total


def stage_b_worker(bounds):
    start, end = bounds
    total = 0.0

    for i in range(start, end):
        x = (i % 1000 + 1) * 0.001

        # 30 arithmetic/trigonometric operations per element.
        for _ in range(10):
            x = (
                x * 1.000001
                + math.sin(x) * 0.0001
                + math.sqrt(abs(x) + 1) * 0.00001
            )

        total += x

    return total


def make_chunks(n, p):
    return [
        (n * i // p, n * (i + 1) // p)
        for i in range(p)
    ]


def measure(pool, p, n):
    samples = []
    serial_samples = []
    checksum = None

    chunks = make_chunks(n, p)

    for _ in range(RUNS):
        t0 = time.perf_counter()

        a0 = time.perf_counter()
        result_a = stage_a(n)
        serial_time = time.perf_counter() - a0

        result_b = pool.map(stage_b_worker, chunks)
        elapsed = time.perf_counter() - t0

        checksum = result_a + sum(result_b)
        samples.append(elapsed)
        serial_samples.append(serial_time)

    return {
        "p": p,
        "N": n,
        "time": median(samples),
        "stage_a": median(serial_samples),
        "checksum": checksum
    }


def run_scaling(p_values, weak=False):
    results = []

    for p in p_values:
        n = p * N0_WEAK if weak else N_STRONG

        with mp.Pool(processes=p) as pool:
            result = measure(pool, p, n)

        results.append(result)

        print(
            f"p={p:2d} | N={n:,} | "
            f"T={result['time']:.4f}s | "
            f"StageA={result['stage_a']:.4f}s",
            flush=True
        )

    return results


def write_csv(filename, rows):
    with open(filename, "w", newline="") as f:
        writer = csv.DictWriter(
            f, fieldnames=list(rows[0].keys())
        )
        writer.writeheader()
        writer.writerows(rows)


def main():
    print("=== PHASE 2 ===", flush=True)

    print("\n--- STRONG SCALING ---", flush=True)
    strong = run_scaling(range(1, P_LOG + 1))

    baseline = strong[0]["time"]
    s = strong[0]["stage_a"] / baseline
    amdahl_cap = 1 / s if s else float("inf")

    for r in strong:
        p = r["p"]
        r["speedup"] = baseline / r["time"]
        r["efficiency"] = r["speedup"] / p
        print(
            f"p={p} | S={r['speedup']:.4f} | "
            f"E={r['efficiency']:.4f}",
            flush=True
        )

    print(f"Amdahl fraction: {s:.6f}")
    print(f"Amdahl cap: {amdahl_cap:.4f}")
    write_csv("phase2_strong.csv", strong)

    print("\n--- WEAK SCALING ---", flush=True)
    weak = run_scaling(range(1, P_LOG + 1), weak=True)
    weak_baseline = weak[0]["time"]

    for r in weak:
        p = r["p"]
        r["weak_efficiency"] = weak_baseline / r["time"]
        r["alpha"] = r["stage_a"] / r["time"]
        r["gustafson"] = p - r["alpha"] * (p - 1)

        print(
            f"p={p} | WeakE={r['weak_efficiency']:.4f} | "
            f"Gustafson={r['gustafson']:.4f}",
            flush=True
        )

    write_csv("phase2_weak.csv", weak)

    print("\n--- VERIFICATION ---", flush=True)
    print(
        emit_verify_tag(
            "PHASE2",
            STUDENT_ID,
            strong[-1]["speedup"],
            weak[-1]["gustafson"]
        )
    )

    print("\nResults saved.")
    print("phase2_strong.csv")
    print("phase2_weak.csv")


if __name__ == "__main__":
    mp.freeze_support()
    main()

```

### task3_roofline_gemm.py

```python
import csv
import math
import multiprocessing as mp
import time
from multiprocessing import shared_memory
from statistics import median

from harness import emit_verify_tag

STUDENT_ID = 230103316
P = 8
N = 256
MEM_BYTES = 64_000_000
FLOPS_PER_WORKER = 60_000_000
TILES = [8, 16, 32, 64, 96, 128]
RUNS = 3

L1_BYTES = 65_536
L2_BYTES = 4_194_304


def bandwidth_worker(args):
    source_name, dest_name, start, end = args
    source = shared_memory.SharedMemory(name=source_name)
    dest = shared_memory.SharedMemory(name=dest_name)

    try:
        src = source.buf[start:end]
        dst = dest.buf[start:end]
        try:
            dst[:] = src
        finally:
            del src, dst
    finally:
        source.close()
        dest.close()


def compute_worker(_):
    # 16 independent scalar multiply-add chains.
    # 16 * 2 FLOPs per iteration.
    a0 = a1 = a2 = a3 = 1.0
    a4 = a5 = a6 = a7 = 1.0
    a8 = a9 = a10 = a11 = 1.0
    a12 = a13 = a14 = a15 = 1.0

    iterations = FLOPS_PER_WORKER // 32

    for _ in range(iterations):
        a0 = a0 * 1.00000001 + 0.00000001
        a1 = a1 * 1.00000001 + 0.00000001
        a2 = a2 * 1.00000001 + 0.00000001
        a3 = a3 * 1.00000001 + 0.00000001
        a4 = a4 * 1.00000001 + 0.00000001
        a5 = a5 * 1.00000001 + 0.00000001
        a6 = a6 * 1.00000001 + 0.00000001
        a7 = a7 * 1.00000001 + 0.00000001
        a8 = a8 * 1.00000001 + 0.00000001
        a9 = a9 * 1.00000001 + 0.00000001
        a10 = a10 * 1.00000001 + 0.00000001
        a11 = a11 * 1.00000001 + 0.00000001
        a12 = a12 * 1.00000001 + 0.00000001
        a13 = a13 * 1.00000001 + 0.00000001
        a14 = a14 * 1.00000001 + 0.00000001
        a15 = a15 * 1.00000001 + 0.00000001

    return sum((
        a0, a1, a2, a3, a4, a5, a6, a7,
        a8, a9, a10, a11, a12, a13, a14, a15
    ))


def gemm_worker(args):
    aname, bname, cname, n, start, end, variant, bs = args

    sa = shared_memory.SharedMemory(name=aname)
    sb = shared_memory.SharedMemory(name=bname)
    sc = shared_memory.SharedMemory(name=cname)

    a = b = c = None

    try:
        a = sa.buf.cast("d")
        b = sb.buf.cast("d")
        c = sc.buf.cast("d")

        if variant == 1:
            for i in range(start, end):
                row = i * n
                for j in range(n):
                    acc = 0.0
                    for k in range(n):
                        acc += a[row + k] * b[k * n + j]
                    c[row + j] = acc

        elif variant == 2:
            for i in range(start, end):
                row = i * n
                for k in range(n):
                    aik = a[row + k]
                    bk = k * n
                    for j in range(n):
                        c[row + j] += aik * b[bk + j]

        elif variant == 3:
            for ii in range(start, end, bs):
                imax = min(ii + bs, end)
                for jj in range(0, n, bs):
                    jmax = min(jj + bs, n)
                    for kk in range(0, n, bs):
                        kmax = min(kk + bs, n)
                        for i in range(ii, imax):
                            row = i * n
                            for k in range(kk, kmax):
                                aik = a[row + k]
                                bk = k * n
                                for j in range(jj, jmax):
                                    c[row + j] += aik * b[bk + j]
        else:
            raise ValueError("Invalid variant")
    finally:
        del a, b, c
        sa.close()
        sb.close()
        sc.close()


def bench(pool, worker, arguments):
    samples = []
    for _ in range(RUNS):
        t0 = time.perf_counter()
        pool.map(worker, arguments)
        samples.append(time.perf_counter() - t0)
    return median(samples)


def write_csv(filename, rows):
    with open(filename, "w", newline="") as f:
        writer = csv.DictWriter(
            f, fieldnames=list(rows[0].keys())
        )
        writer.writeheader()
        writer.writerows(rows)


def main():
    print("=== PHASE 3 ===", flush=True)

    # 3A: Memory bandwidth.
    print("\n--- BANDWIDTH ---", flush=True)

    src = shared_memory.SharedMemory(
        create=True, size=MEM_BYTES
    )
    dst = shared_memory.SharedMemory(
        create=True, size=MEM_BYTES
    )

    try:
        # Initialize the full source buffer.
        src.buf[:MEM_BYTES] = bytes([1]) * MEM_BYTES

        chunk = MEM_BYTES // P
        arguments = [
            (src.name, dst.name,
             i * chunk,
             MEM_BYTES if i == P - 1 else (i + 1) * chunk)
            for i in range(P)
        ]

        with mp.Pool(P) as pool:
            pool.map(bandwidth_worker, arguments)
            bw_time = bench(
                pool, bandwidth_worker, arguments
            )

            bandwidth = 2 * MEM_BYTES / bw_time / 1e9

            print(
                f"Bandwidth: {bandwidth:.4f} GB/s",
                flush=True
            )

            print("\n--- COMPUTE CEILING ---", flush=True)

            tasks = list(range(P))
            pool.map(compute_worker, tasks)
            compute_time = bench(
                pool, compute_worker, tasks
            )

            peak = (
                FLOPS_PER_WORKER * P
                / compute_time / 1e9
            )
            ridge = peak / bandwidth

            print(
                f"Compute: {peak:.4f} GFLOP/s",
                flush=True
            )
            print(
                f"Ridge: {ridge:.4f} FLOP/byte",
                flush=True
            )
    finally:
        src.close()
        src.unlink()
        dst.close()
        dst.unlink()

    # 3B: GEMM.
    print("\n--- GEMM ---", flush=True)

    matrix_bytes = N * N * 8
    sa = shared_memory.SharedMemory(
        create=True, size=matrix_bytes
    )
    sb = shared_memory.SharedMemory(
        create=True, size=matrix_bytes
    )
    sc = shared_memory.SharedMemory(
        create=True, size=matrix_bytes
    )

    try:
        a = sa.buf.cast("d")
        b = sb.buf.cast("d")

        for i in range(N * N):
            a[i] = ((i % 97) - 48) / 97.0
            b[i] = ((i % 89) - 44) / 89.0

        del a, b

        partitions = [
            (N * i // P, N * (i + 1) // P)
            for i in range(P)
        ]

        cases = [(1, 0), (2, 0)] + [
            (3, bs) for bs in TILES
        ]

        rows = []
        outputs = {}

        with mp.Pool(P) as pool:
            for variant, bs in cases:
                args = [
                    (
                        sa.name, sb.name, sc.name,
                        N, start, end, variant, bs
                    )
                    for start, end in partitions
                ]

                samples = []

                for _ in range(RUNS):
                    sc.buf[:] = bytes(matrix_bytes)
                    t0 = time.perf_counter()
                    pool.map(gemm_worker, args)
                    samples.append(
                        time.perf_counter() - t0
                    )

                elapsed = median(samples)
                gflops = 2 * N**3 / elapsed / 1e9

                footprint = (
                    3 * bs * bs * 8 if variant == 3 else 0
                )
                intensity = (
                    bs / 12 if variant == 3 else 0
                )

                roofline = (
                    min(peak, bandwidth * intensity)
                    if variant == 3 else None
                )

                row = {
                    "variant": variant,
                    "tile_size": bs,
                    "median_seconds": elapsed,
                    "gflops": gflops,
                    "tile_bytes": footprint,
                    "intensity": intensity,
                    "roofline_gflops": roofline,
                    "fits_L1": (
                        footprint <= L1_BYTES
                        if variant == 3 else ""
                    ),
                    "fits_L2": (
                        footprint <= L2_BYTES
                        if variant == 3 else ""
                    )
                }
                rows.append(row)

                view = sc.buf.cast("d")
                outputs[(variant, bs)] = list(view)
                del view

                print(
                    f"V={variant} BS={bs:3d} "
                    f"T={elapsed:.4f}s "
                    f"GFLOP/s={gflops:.5f}",
                    flush=True
                )

        # Compare both reordered and blocked GEMM
        # against naive GEMM.
        reference = outputs[(1, 0)]

        print("\n--- ACCURACY ---", flush=True)

        for key, values in outputs.items():
            error = max(
                abs(x - y)
                for x, y in zip(reference, values)
            )
            passed = error < 1e-9
            print(
                f"Variant={key[0]} BS={key[1]} "
                f"Error={error:.3e} PASS={passed}",
                flush=True
            )
            if not passed:
                raise RuntimeError("GEMM verification failed")

        blocked = [
            r for r in rows if r["variant"] == 3
        ]
        best = max(blocked, key=lambda r: r["gflops"])

        print("\n--- ROOFLINE COMPARISON ---", flush=True)

        for r in blocked:
            print(
                f"BS={r['tile_size']:3d} "
                f"Tile={r['tile_bytes']}B "
                f"L1={r['fits_L1']} "
                f"L2={r['fits_L2']} "
                f"Measured={r['gflops']:.5f} "
                f"Roofline={r['roofline_gflops']:.5f}",
                flush=True
            )

        print(f"\nBest BS: {best['tile_size']}", flush=True)

        write_csv("phase3_results.csv", rows)
        write_csv(
            "phase3_roofline.csv",
            [
                {"metric": "bandwidth_GBs", "value": bandwidth},
                {"metric": "compute_GFLOPs", "value": peak},
                {"metric": "ridge_FLOP_per_byte", "value": ridge}
            ]
        )

        print("\n--- VERIFICATION ---", flush=True)
        print(
            emit_verify_tag(
                "PHASE3",
                STUDENT_ID,
                rows[0]["gflops"],
                best["gflops"]
            ),
            flush=True
        )

        print("\nResults saved.", flush=True)

    finally:
        sa.close()
        sa.unlink()
        sb.close()
        sb.unlink()
        sc.close()
        sc.unlink()


if __name__ == "__main__":
    mp.freeze_support()
    main()

```

### task4_scheduling.py

```python
import csv
import math
import multiprocessing as mp
import time

from harness import emit_verify_tag

STUDENT_ID = 230103316
N = 8000
P = 8
CHUNKS = [1, 8, 64, 512]


def heavy_work(i):
    iterations = 1 + (i * i) // 1600
    x = (i + 1) * 0.0001

    for _ in range(iterations):
        x = (
            math.sin(x)
            + 0.5 * math.cos(x)
            + 0.01 * math.sqrt(abs(x) + 1.0)
        )

    return x


def static_worker(worker_id, strategy, p, n,
                  active_times, checksums):
    t0 = time.perf_counter()
    result = 0.0

    if strategy == "Block Static":
        start = n * worker_id // p
        end = n * (worker_id + 1) // p
        indices = range(start, end)
    else:
        indices = range(worker_id, n, p)

    for i in indices:
        result += heavy_work(i)

    active_times[worker_id] = (
        time.perf_counter() - t0
    )
    checksums[worker_id] = result


def dynamic_worker(worker_id, n, chunk,
                   counter, active_times,
                   checksums):
    t0 = time.perf_counter()
    result = 0.0

    while True:
        with counter.get_lock():
            start = counter.value
            counter.value += chunk

        if start >= n:
            break

        end = min(start + chunk, n)

        for i in range(start, end):
            result += heavy_work(i)

    active_times[worker_id] = (
        time.perf_counter() - t0
    )
    checksums[worker_id] = result


def run_case(strategy, chunk=0):
    active_times = mp.Array("d", P)
    checksums = mp.Array("d", P)
    counter = mp.Value("i", 0)

    processes = []

    for worker_id in range(P):
        if strategy == "Dynamic":
            target = dynamic_worker
            args = (
                worker_id, N, chunk, counter,
                active_times, checksums
            )
        else:
            target = static_worker
            args = (
                worker_id, strategy, P, N,
                active_times, checksums
            )

        process = mp.Process(
            target=target,
            args=args
        )
        processes.append(process)

    t0 = time.perf_counter()

    for process in processes:
        process.start()

    for process in processes:
        process.join()

    wall_time = time.perf_counter() - t0

    for process in processes:
        if process.exitcode != 0:
            raise RuntimeError(
                f"Worker failed: {process.exitcode}"
            )

    worker_times = list(active_times)
    total_active = sum(worker_times)

    mean_active = total_active / P
    imbalance = (
        max(worker_times) / mean_active
        if mean_active > 0 else 0
    )

    return {
        "strategy": strategy,
        "chunk": chunk,
        "wall_seconds": wall_time,
        "imbalance_lambda": imbalance,
        "min_worker_seconds": min(worker_times),
        "max_worker_seconds": max(worker_times),
        "checksum": sum(checksums)
    }


def main():
    print("=== PHASE 4 ===", flush=True)
    print(f"CPU workers: {P}", flush=True)
    print(f"Workload items: {N}", flush=True)

    cases = [
        ("Block Static", 0),
        ("Cyclic Static", 0)
    ] + [
        ("Dynamic", c) for c in CHUNKS
    ]

    results = []

    print("\n--- SCHEDULING TESTS ---", flush=True)

    for strategy, chunk in cases:
        print(
            f"Running {strategy}, chunk={chunk} ...",
            flush=True
        )

        r = run_case(strategy, chunk)
        results.append(r)

        print(
            f"{strategy:13s} | "
            f"C={chunk:3d} | "
            f"T={r['wall_seconds']:.4f}s | "
            f"lambda={r['imbalance_lambda']:.4f}",
            flush=True
        )

    print("\n--- VALIDATION ---", flush=True)

    reference = results[0]["checksum"]

    for r in results:
        difference = abs(
            r["checksum"] - reference
        )
        print(
            f"{r['strategy']} C={r['chunk']} | "
            f"Checksum difference={difference:.6e}",
            flush=True
        )

    dynamic_results = [
        r for r in results
        if r["strategy"] == "Dynamic"
    ]

    best = min(
        dynamic_results,
        key=lambda r: r["wall_seconds"]
    )

    block = results[0]
    improvement = (
        block["wall_seconds"] / best["wall_seconds"]
    )

    print("\n--- BEST DYNAMIC CHUNK ---", flush=True)
    print(f"Optimal C*: {best['chunk']}", flush=True)
    print(
        f"Best dynamic time: "
        f"{best['wall_seconds']:.4f}s",
        flush=True
    )
    print(
        f"Speedup vs Block Static: "
        f"{improvement:.4f}x",
        flush=True
    )

    with open(
        "phase4_results.csv",
        "w",
        newline=""
    ) as f:
        writer = csv.DictWriter(
            f,
            fieldnames=list(results[0].keys())
        )
        writer.writeheader()
        writer.writerows(results)

    print("\n--- VERIFICATION ---", flush=True)

    print(
        emit_verify_tag(
            "PHASE4",
            STUDENT_ID,
            block["wall_seconds"],
            best["wall_seconds"]
        ),
        flush=True
    )

    print(
        "\nResults saved to phase4_results.csv",
        flush=True
    )


if __name__ == "__main__":
    mp.freeze_support()
    main()

```
