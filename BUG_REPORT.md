# Bug Report for MATLAB Acoustic Channel Simulator

## Summary
This document lists all bugs found and fixed in the MATLAB acoustic channel simulator codebase.

## Bugs Fixed

### 1. Fortran-style Double Precision Notation (CRITICAL)
**Files affected:** `step.m`, `crci.m`, `reducestep.m`, `scalep.m`
**Severity:** HIGH - Code may not run correctly in MATLAB

**Description:**
The code used Fortran-style double precision notation (`d0`, `d-`, `d+`) which is not recognized by MATLAB. MATLAB uses `e` for exponential notation, not `d`.

**Examples:**
- `step.m` line 39-40: `2.0d0` → `2.0`, `1.0d0` → `1.0`
- `crci.m` line 50: `3.3d-3` → `3.3e-3`, `3d-4` → `3e-4`
- `reducestep.m` line 67-68: `1.0d-4` → `1.0e-4`, `1.0d-5` → `1.0e-5`
- `scalep.m` line 29: Fixed in comments for consistency

**Impact:** 
- MATLAB may interpret `d0` as variable concatenation rather than literal value
- Could cause syntax errors or incorrect numerical values
- Critical for accurate numerical computations

**Fix:** Changed all Fortran notation to MATLAB standard notation using `e`.

### 2. Undefined Variables in Reflection Code (CRITICAL)
**File affected:** `reflect.m`
**Severity:** HIGH - Runtime error when using file-based boundary conditions

**Description:**
Line 57 uses undefined variables `rInt` and `phiInt`. The function call that should define these variables (line 56) is commented out, but the usage is not.

**Code:**
```matlab
case ( 'F' )                 % file
    theInt = RADDEG * abs( atan2( Th, Tg ) );
    %[ theta, RefC, phi ] = RefCO( theInt, rInt, phiInt, Npts  )
    ray( I ).Rfa = ray( I ).Rfa * rInt * exp( 1i * phiInt );  % BUG: rInt, phiInt not defined!
```

**Impact:**
- Runtime error: "Undefined function or variable 'rInt'"
- Code cannot handle file-based reflection boundary conditions
- Feature is non-functional

**Fix:** Added error message indicating feature is not implemented and requires RefCO function.

## Known Issues (Not Fixed)

### 3. Missing External Dependency
**File affected:** `channel_simulator.m`
**Severity:** HIGH - Code will fail at runtime

**Description:**
Line 287 calls `find_takagi_factor()` which is not included in the repository.

**Code:**
```matlab
error_takagi=-1;
while sign(error_takagi)~=1;
    [Wp_Q, Wp_s, error_takagi]= find_takagi_factor(Wp_cov);  % Missing function!
end
```

**Impact:**
- Runtime error when using statistically equivalent channel model
- Potential infinite loop if function doesn't converge
- Requires external Takagi factorization package

**Recommendation:** Include the Takagi factorization implementation or add dependency documentation.

### 4. Potential Division by Zero
**File affected:** `reflect.m`
**Severity:** MEDIUM - Edge case handling

**Description:**
Lines 25 and 32 divide by `Th` (ray tangent component normal to boundary) without checking for zero.

**Code:**
```matlab
RN = 2 * kappa / c^2 / Th;    % Division by Th
RM = Tg ./ Th;                 % Division by Th
```

**Impact:**
- Division by zero for grazing incidence (ray parallel to boundary)
- Results in Inf/NaN values
- Known limitation in ray-tracing methods

**Recommendation:** Add check for |Th| < epsilon and handle grazing case specially.

### 5. Unconventional Notation
**File affected:** `reflect.m`
**Severity:** LOW - Cosmetic issue

**Description:**
Line 61 uses `c'` (transpose of scalar) which is unconventional but not incorrect.

**Code:**
```matlab
gamma1SQ = ( omega / c'  ).^ 2 - GK^ 2;
```

**Impact:** None - for real scalars, `c' == c`
**Recommendation:** Remove unnecessary transpose for clarity.

## Testing Recommendations

1. Test all fixed files with typical simulation parameters
2. Verify numerical accuracy hasn't changed (except for Fortran notation fixes)
3. Test error handling for unimplemented features
4. Add unit tests for boundary reflection cases
5. Document external dependencies (Takagi factorization)

## Files Modified

- `step.m` - Fixed Fortran notation
- `crci.m` - Fixed Fortran notation  
- `reducestep.m` - Fixed Fortran notation
- `scalep.m` - Fixed Fortran notation in comments
- `reflect.m` - Fixed undefined variables, added error handling
- `.gitignore` - Added to ignore system files

## Commits

1. `53f8ecc` - Fix Fortran-style notation bugs in MATLAB code
2. `10138dc` - Fix undefined variables bug in reflect.m
