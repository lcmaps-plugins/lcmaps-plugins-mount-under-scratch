# lcmaps-plugins-mount-under-scratch

![](https://api.travis-ci.org/lcmaps-plugins/lcmaps-plugins-mount-under-scratch.svg?branch=master)

The "lcmaps-plugins-mount-under-scratch" plugin allows the admin to setup
per-invocation copies of world-writable directories such as `/tmp` or `/var/tmp`.

This plugin will create a temporary directory of the form:

```
/tmp/glexec_mount_under_scratch_XXXXXX
```

and subdirectories

```
/tmp/glexec_mount_under_scratch_MuLdpO:
    tmp/
    var/tmp
```

owned by the payload user.

Further, `/tmp` will map to `/tmp/glexec_mount_under_scratch_MuLdpO/tmp`
and /var/tmp will map to /tmp/glexec_mount_under_scratch_MuLdpO/var/tmp for
the payload process (and *only* the payload process).

If /tmp or /var/tmp are on a shared-bind-mount (meaning the parent and child
namespace are identical for a subtree), the most specific subtree will be
remounted as private.

The above paragraphs are similar for admin-defined directories.  The admin can
pass:

```
-path /var/tmp
-path /foo
-path /bar
-path /tmp
```

as arguments in lcmaps.db to the plugin to remap /var/tmp, /tmp, /foo, and
/bar.  Note that order does matter!  As the source of the mount is in /tmp,
/tmp must be listed last in any set of mappings.

If an admin passes one path, then /tmp and /var/tmp are not done by default.

This plugin takes its inspiration from the implementation of the
MOUNT_UNDER_SCRATCH parameter in Condor, and is necessary for
MOUNT_UNDER_SCRATCH to work with glexec'd jobs.  Note the plugin does not
cleanup the created directories - that is the responsibility of the invoker;
note that this limitation means this plugin only makes sense when /tmp and
/var/tmp are job-specific already (i.e., when used with MOUNT_UNDER_SCRATCH).

This code is licensed under the Apache 2.0 license, and is copyrighted by the
University of Nebraska-Lincoln.

Some aspects are derived from the Condor source code, copyrighted Red Hat or
University of Wisconsin (and contributed by me under the Condor Contributor
Licensing Agreement) and also licensed under Apache 2.0.

Enjoy!

Brian Bockelman

