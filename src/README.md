# roche
<table width="100%">
<tr><td align="center">
<img src="https://raw.githubusercontent.com/roche-rs/website/main/icons/saturn.svg" width="200" height="200"> 
</td>
</tr>
</table>

**Roche is a commandline tool for building rust http and event services using containers.**

To see how it works it's recommended to walk through the [simple tutorial](/tutorials/simple.md).

## motivation

Services built with Rust have some fantastic runtime qualities for serverless/container scenarios:

* low resource footprint 

* quick startup time 

* zero garbage collection

However these things come with a trade off as build times are not ideal for rapid application development.

roche addresses this short coming by providing a function as a service pattern for [tide](https://github.com/http-rs/tide) that reduces build times to seconds and enables developers to focus on business logic allowing for the speedy delivery of blazing fast and energy efficient software.

It leverages the [nesting feature of tide](https://github.com/http-rs/tide/blob/main/examples/nested.rs) so all that is required to be developed is a handler while the application infrastructure is provided by prebuilt docker containers.

Once the base images are downloaded build times are around 5s for debug and 30s for Release.

Roche is intended to target [knative environments](https://knative.dev/docs/knative-offerings/) but it can also be used to build standard docker containers.

## comparisons

There are other opensource tools in this space that provide similar functionality.
As they mature converging may be appropriate but currently they have differing constraints and target audiences.

<table>
<tr>
<th>Name</th><th>tag line</th><th>lang</th><th>notes</th></tr>
<tr>
<td>
<a href="https://roche-rs.org/">roche</a>
</td>
<td>
Roche is a commandline tool for building rust http and event services using containers.
</td>
<td>
rust
</td>
<td>
Targeted Rust use-cases.<br/>Smaller developer pool<br/>http-rs specific
</td>
</tr>
<tr>
<td>
<a href="https://odo.dev/">odo</a>
</td>
<td>
odo is a fast, iterative, and straightforward CLI tool for developers who write, build, and deploy applications on Kubernetes and OpenShift.
</td>
<td>
go
</td>
<td>
Focus on multilanguage and enterprise scenarios but no official rust support.<br/>Devfiles specification is still being defined.
</td>
</tr>
<tr>
<td>
<a href="https://github.com/boson-project/func">boson-project</a>
</td>
<td>
Function as a Service CLI and Client Libray for Knative-enabled Clusters.
</td>
<td>
go
</td>
<td>
Multilanguage but no rust yet<br/>includes knative management<br/>Lacks granular build management
</td>
</tr>
<table>