# Noise-dependent-utility-and-failure-boundaries-of-THNR-guided-VMD-
THNR-guided VMD mode-subset selection for bearing fault feature extraction: code, data, and reproducibility package for the JVC paper on noise-dependent utility and failure boundaries.
# THNR-guided VMD Mode-Subset Selection

This repository contains the code, frozen configuration, and result data for:

> Zhi, T., Zhao, D., Jiang, H., Zhang, X., Yan, X., & Guo, J. (2026).
> Noise-dependent utility and failure boundaries of THNR-guided VMD
> mode-subset selection for bearing fault feature extraction.
> *Journal of Vibration and Control*.

## Quick Start

```matlab
cd code
run_reproduction_smoke   % verifies self-contained execution

# Submission Release Package v2

This package contains the complete code, configuration, and result data supporting the manuscript:

**"Noise-dependent utility and failure boundaries of THNR-guided VMD mode-subset selection for bearing fault feature extraction"**

## Directory Structure

```
submission_release_v2/
├── code/                                    # MATLAB source code
│   ├── user_methods/                        # Method implementations
│   │   ├── method_proposed.m               # Proposed method (variant E)
│   │   ├── method_fixed_vmd.m              # Fixed VMD baseline
│   │   ├── method_se_gini_vmd.m            # SE-Gini VMD baseline (variant C)
│   │   ├── method_fast_kurtogram.m         # Fast Kurtogram baseline
│   │   ├── method_autogram.m               # Autogram baseline
│   │   ├── ar_whiten.m                     # AR pre-whitening for Autogram
│   │   ├── kurtogram_bands.m               # 1/3-binary-tree kurtogram
│   │   └── locate_frozen_dir.m             # Locate frozen_v21 on path
│   ├── frozen_v21/                         # Core algorithm functions (frozen v2.1)
│   │   ├── fz_gini_recall.m                # SES-Gini band recall
│   │   ├── fz_bandpass_mask.m              # Frequency-domain bandpass
│   │   ├── fz_vmd.m                        # Variational mode decomposition
│   │   ├── fz_mode_subset_select.m         # THNR exhaustive subset selection
│   │   ├── fz_thnr.m                       # THNR computation
│   │   ├── fz_amp_env_spectrum.m           # Amplitude-envelope spectrum
│   │   ├── fz_defaults.m                   # Default parameters
│   │   ├── fz_ses_gini.m                   # SES-Gini computation
│   │   ├── fz_sim_signal.m                 # Synthetic signal generator
│   │   ├── fz_sequential_optimize.m        # K-alpha optimization (F/G only)
│   │   ├── fz_pipeline.m                   # Full pipeline (F/G only)
│   │   ├── fz_kinematic_ff.m               # Bearing fault frequency calculator
│   │   ├── fz_norm_env_half.m              # Envelope normalization
│   │   └── ... (70 files total)
│   ├── config_experiments.m                # Experiment configuration
│   ├── frozen_parameters.mat               # Frozen parameter file
│   ├── build_independent_evidence.m        # Semi-physical injection experiment
│   ├── run_confirmation_bcde.m             # Independent confirmation experiment
│   ├── run_oracle_diagnosis.m              # Oracle subset diagnosis
│   ├── verify_confirmation_equivalence.m   # Numerical equivalence check
│   ├── run_reproduction_smoke.m            # Clean-environment smoke test
│   ├── confirmation_protocol_v1.md         # Pre-specified confirmation protocol
│   └── MATLAB_VERSION.md                   # MATLAB and toolbox requirements
├── results/
│   ├── production_20261006_161001/         # Official S1, S3, S4, S5 results
│   ├── production_20261003_145120_S2_ablation/  # S2 ablation results
│   ├── confirmation/                       # Independent-seed confirmation results
│   ├── oracle/                             # Oracle subset analysis results
│   └── independent_evidence/               # Semi-physical injection + normal-record tests
├── README.md                               # This file
└── sha256_manifest.csv                     # SHA-256 checksums for all files
```

## Key Files

### Core Method (variant E)

- `code/user_methods/method_proposed.m`: Implements variant E
  - SES-Gini top-1 band recall (blind stage)
  - Fixed K=4 VMD with bandwidth-normalized α₀ = 200·f_s/BW_selected
  - Exhaustive 15-subset THNR selection (prior-guided stage)
- `code/frozen_v21/fz_thnr.m`: THNR using amplitude-envelope spectrum power P(f)=A²(f)
- `code/frozen_v21/fz_amp_env_spectrum.m`: Amplitude-envelope spectrum (not squared-envelope)
- `code/frozen_v21/fz_mode_subset_select.m`: Exhaustive non-empty subset enumeration

### Configuration

- `code/frozen_parameters.mat`: B=1, H=3, df_tol_hz=3.0, lambda_K=0.5, K_list=[2,3,4,5]
- `code/config_experiments.m`: Master experiment configuration

### Results

- `results/production_20261006_161001/`: S1 (20-cell simulation), S3 (XJTU-SY), S4 (frequency bias), S5 (CWRU)
- `results/production_20261003_145120_S2_ablation/`: S2 ablation (B/C/D/E/F/G variants)
- `results/confirmation/`: S1-confirm (1000 signals, seed 30261008), E-C paired statistics
- `results/oracle/`: Oracle subset analysis (15 subsets per signal)
- `results/independent_evidence/`: Semi-physical injection (24 mixtures) and normal-record hypothesis tests

## Quick Start: Reproduction Smoke Test

To verify the package is self-contained and runs without the original project tree:

```matlab
% 1. Extract the ZIP to any directory (e.g., C:\temp\release_test)
% 2. Open MATLAB and navigate to the release root
cd C:\temp\release_test\submission_release_v2

% 3. Run the smoke test
run_reproduction_smoke
```

Expected output: `ALL CHECKS PASSED`

The smoke test:

1. Adds `code/`, `code/frozen_v21/`, and `code/user_methods/` to the path
2. Verifies all 13 required functions are available
3. Generates a synthetic bearing fault signal (1 s, fs=25600, BPFO=105 Hz, -10 dB SNR)
4. Runs variant E end-to-end
5. Checks output structure and signal length

## Full Reproduction

### Confirmation Experiment

```matlab
cd code
run_confirmation_bcde   % 1000 signals, 4 noise types x 5 SNRs x 50 reps
```

### Oracle Diagnosis

```matlab
cd code
run_oracle_diagnosis    % 15-subset oracle analysis on confirmation set
```

### Semi-Physical Independent Evidence

```matlab
cd code
build_independent_evidence   % 24 injection mixtures + normal-record tests
```

Note: Requires CWRU normal baseline records (publicly available).

### Numerical Equivalence Check

```matlab
cd code
verify_confirmation_equivalence
```

## Reproducibility Notes

- **S1** is the exploratory development set; **S1-confirm** is the independent-seed confirmation set (same signal model, disjoint random seeds).
- The method configuration was frozen before the confirmation set was generated.
- Oracle analysis is diagnostic only and is **not** a deployable method.
- CWRU and XJTU-SY are publicly available datasets; raw data are not included here.
- The proposed method uses variant E only. Variants F/G were rejected after ablation and are retained in `frozen_v21/` for historical reference only.
- All result files have SHA-256 checksums in `sha256_manifest.csv`.

## Version History

- **v2 (current)**: Added S2 ablation results, independent evidence outputs,
  complete frozen_v21 dependency tree, user_methods baselines,
  `run_reproduction_smoke.m`, `MATLAB_VERSION.md`, and corrected README paths.
- **v1**: Initial release with S1/S3/S4/S5, confirmation, and oracle results.

