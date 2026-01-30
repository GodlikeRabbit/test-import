## Goal                                                                                                                                                                                 
                                                                                                                                                                                          
  Get the compiler to build on modern GCC/Clang with minimal changes. Suppress warnings rather than fix them.                                                                             
                                                                                                                                                                                          
  ## Strategy                                                                                                                                                                             
                                                                                                                                                                                          
  Modern C compilers (GCC 10+, Clang 12+) treat many old C patterns as errors by default. We'll use compiler flags to downgrade these to warnings, and add only the minimal code          
  changes needed.                                                                                                                                                                         
                                                                                                                                                                                          
  ## Implementation Steps                                                                                                                                                                 
                                                                                                                                                                                          
  ### Step 1: Update Makefiles                                                                                                                                                            
                                                                                                                                                                                          
  Add flags to suppress errors from old C patterns. Update these files:                                                                                                                   
                                                                                                                                                                                          
  **Files to modify:**                                                                                                                                                                    
  - `src/front/makefile` - change `-w` to modern warning suppression                                                                                                                      
  - `src/veyacc/makefile`                                                                                                                                                                 
  - `src/libvy/makefile`                                                                                                                                                                  
  - `src/view/makefile`                                                                                                                                                                   
  - `src/codgen1/makefile`                                                                                                                                                                
  - `src/codgen2/makefile`                                                                                                                                                                
  - `src/rt/makefile`                                                                                                                                                                     
  - `src/adadep/makefile`                                                                                                                                                                 
  - `src/amdb/makefile`                                                                                                                                                                   
  - `src/h/makefile`                                                                                                                                                                      
  - `src/standard/makefile`                                                                                                                                                               
  - `src/treetool/makefile`                                                                                                                                                               
                                                                                                                                                                                          
  **New CFLAGS:**                                                                                                                                                                         
  ```makefile                                                                                                                                                                             
  CC = gcc                                                                                                                                                                                
  CFLAGS = -std=gnu89 -O2 -w -Wno-error \                                                                                                                                                 
  -Wno-implicit-int \                                                                                                                                                                     
  -Wno-implicit-function-declaration \                                                                                                                                                    
  -Wno-return-type \                                                                                                                                                                      
  -Wno-int-conversion \                                                                                                                                                                   
  -Wno-incompatible-pointer-types \                                                                                                                                                       
  -Wno-builtin-declaration-mismatch                                                                                                                                                       
  ```                                                                                                                                                                                     
                                                                                                                                                                                          
  Using `-std=gnu89` allows K&R function definitions and implicit int.                                                                                                                    
                                                                                                                                                                                          
  ### Step 2: Fix Critical Compilation Errors                                                                                                                                             
                                                                                                                                                                                          
  Some issues cause hard errors even with warning suppression:                                                                                                                            
                                                                                                                                                                                          
  1. **`#endif` with trailing tokens** - Fix in header files:                                                                                                                             
  - `src/h/rt_defs.h` - change `#endif EM` to `#endif /* EM */`                                                                                                                           
  - Other headers as needed                                                                                                                                                               
                                                                                                                                                                                          
  2. **Conflicting NULL definitions** - `src/h/tree.h` defines `#define NULL 0` which conflicts with system headers. Guard it:                                                            
  ```c                                                                                                                                                                                    
  #ifndef NULL                                                                                                                                                                            
  #define NULL 0                                                                                                                                                                          
  #endif                                                                                                                                                                                  
  ```                                                                                                                                                                                     
                                                                                                                                                                                          
  3. **Missing standard includes** - Add where compilation fails:                                                                                                                         
  - `#include <stdlib.h>` for `malloc`, `free`, `exit`                                                                                                                                    
  - `#include <string.h>` for `strcpy`, `strcmp`                                                                                                                                          
  - `#include <unistd.h>` for `unlink`                                                                                                                                                    
                                                                                                                                                                                          
  ### Step 3: Fix Obsolete Function References                                                                                                                                            
                                                                                                                                                                                          
  Replace only if they cause link errors:                                                                                                                                                 
  - `index()` → `strchr()` (may work with `-std=gnu89`)                                                                                                                                   
  - `rindex()` → `strrchr()`                                                                                                                                                              
  - `cfree()` → `free()`                                                                                                                                                                  
                                                                                                                                                                                          
  ### Step 4: Build and Fix Iteratively                                                                                                                                                   
                                                                                                                                                                                          
  1. Run `make` from `src/`                                                                                                                                                               
  2. Fix any hard errors that appear                                                                                                                                                      
  3. Repeat until binaries build                                                                                                                                                          
                                                                                                                                                                                          
  ## Files Summary                                                                                                                                                                        
                                                                                                                                                                                          
  | File | Change |                                                                                                                                                                       
  |------|--------|                                                                                                                                                                       
  | `src/*/makefile` (12 files) | Update CC and CFLAGS |                                                                                                                                  
  | `src/h/tree.h` | Guard NULL definition |                                                                                                                                              
  | `src/h/rt_defs.h` | Fix `#endif` tokens |                                                                                                                                             
  | Various `.c` files | Add missing `#include` as needed |                                                                                                                               
                                                                                                                                                                                          
  ## Verification                                                                                                                                                                         
                                                                                                                                                                                          
  1. `cd src && make`                                                                                                                                                                     
  2. Verify these binaries exist: `veyacc/veyacc`, `front/ada_front`                                                                                                                      
  3. No compilation errors (warnings are acceptable)                                                                                                                                      
                                                                                                                                                                                          
  ## Notes                                                                                                                                                                                
                                                                                                                                                                                          
  - Using `-std=gnu89` preserves K&R compatibility                                                                                                                                        
  - `-w` suppresses all warnings                                                                                                                                                          
  - This approach makes minimal code changes                                                                                                                                              
  - The compiler generates 68000 assembly, so output won't run on modern systems 