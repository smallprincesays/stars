+++
title = "On profiling distributed systems"
date = 2024-12-25
+++

A common pain point I've been having is profiling distributed systems, especially
when the project I'm working on is very latency-sensitive. I decided
to bite the bullet and spend the needed time to set up profiling in Rust.

```rust
impl From<&str> for RTorchNamespace {
    fn from(namespace: &str) -> Self {
        match namespace {
            "torch" => RTorchNamespace::Torch { object: None },
            "torch.Tensor" => RTorchNamespace::Torch {
                object: Some(TorchObject::Tensor),
            },
            "torch.cuda" => RTorchNamespace::Cuda,
            "torch.nn" => RTorchNamespace::Nn { object: None },
            "torch.nn.Module" => RTorchNamespace::Nn {
                object: Some(NnObject::Module),
            },
            "torch.nn.BCELoss" => RTorchNamespace::Nn {
                object: Some(NnObject::BCELoss),
            },
            "torch.optim" => RTorchNamespace::Optim { object: None },
            "torch.optim.Adam" => RTorchNamespace::Optim {
                object: Some(OptimObject::Adam),
            },
            _ => panic!("Invalid namespace"),
        }
    }
}
```
