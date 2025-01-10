Fix Alias 8.5, 9.0 and 9.6 startup crash with virtual memory error
==================================================================
After install Alias 8.5, 9.0 and 9.6, then install Maya or newer version of studio tools, Alias will crash with error "___Insufficient virtual memory (that means swap, not virtual swap)___".

1. Open Software Manager and uninstall Alias|Wavefront 8.5, Alias|Wavefront 9.0, Alias, Alias|Wavefront StudioTools 9.6, Alias|Wavefront Evalviewer 9.0 and Alias|Wavefront Evalviewer 9.6

2. Re-install above software with Software Manager.

3. When finish install, choose "Continue with installations", then install other software related to Alias 8.5, 9.0 and 9.6 and don't exit Software Manager yet.

4. Open Process Manager and terminate swmgr processes.

5. Done!
