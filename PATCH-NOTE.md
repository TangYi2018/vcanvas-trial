# Trial-build patch note (NOT upstream)

Upstream: e01-ai/vcanvas @ 8388a266ed573d9083fb0cf64eaaf852f7de7fff.

Bug found during trial (2026-11-04): the Custom provider's URL field is saved
to provider state (`customEndpoint`) but never passed to any generation request.
All three `streamChat` call sites in `src/App.tsx` omit the `endpointOverride`
argument, and the Custom provider definition has an empty `endpoint`, so every
Custom generation fails with "No endpoint configured. Set a base URL in settings."
Re-entering the URL does not fix it.

Trial-only fix (3 call sites + dependency arrays in `src/App.tsx`): pass
`providerState.customEndpoint` as the `endpointOverride` argument to `streamChat`
(generate, refine, plan-mode). No other source file changed. The deployed files
here are built from this patched copy; evaluate accordingly.
