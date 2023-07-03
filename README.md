# [DisposableObject](https://github.com/myd7349/DisposableObject) [![NuGet](https://img.shields.io/nuget/v/DisposableObject.svg)](https://www.nuget.org/packages/DisposableObject/)

A simple base class for disposable objects.

# Usage

This library provides a base class `DisposableObject` for classes that need to implement the [IDisposable Interface](https://docs.microsoft.com/en-us/dotnet/api/system.idisposable?view=net-6.0).

```csharp
namespace System
{
    public abstract class DisposableObject : IDisposable
    {
        ~DisposableObject()
        {
            Dispose(false);
        }

        public void Dispose()
        {
            Dispose(true);
            GC.SuppressFinalize(this);
        }

        private void Dispose(bool disposing)
        {
            if (!disposed_)
            {
                if (disposing)
                {
                    DisposeManaged();
                }

                DisposeUnmanaged();

                disposed_ = true;
            }
        }

        protected virtual void DisposeManaged()
        {
        }

        protected virtual void DisposeUnmanaged()
        {
        }

        protected void ThrowIfDisposed()
        {
            if (disposed_)
                throw new ObjectDisposedException(string.Format("{0} object is disposed.", GetType().Name));
        }

        protected bool disposed_ = false;
    }
}
```

As shown above, `DisposableObject` provides two virtual methods, `DisposeManaged` and  `DisposeUnmanaged`, to clean up managed and unmanaged resources  respectively. You can override one or both of these methods as needed. 

The `ThrowIfDisposed` method checks whether the current object has been disposed, and if so, throws an exception.

# Known Issues

1. Given that C# does not support multiple inheritance of classes, if your class already includes a base class, it may not be  possible to inherit from `DisposableObject`. In such a case, you can use `DisposableObject` as a reference to implement the [IDisposable Interface](https://docs.microsoft.com/en-us/dotnet/api/system.idisposable?view=net-6.0) on your own.
2. (Feedback is welcome.)

# Related Projects

- [Disposables](https://github.com/StephenCleary/Disposables)
- [Dispose.Scope](https://github.com/InCerryGit/Dispose.Scope)
- [IDisposableAnalyzers](https://github.com/DotNetAnalyzers/IDisposableAnalyzers)
- [Open.Disposable](https://github.com/Open-NET-Libraries/Open.Disposable)
- [System.DisposableObject](https://github.com/porrey/System.DisposableObject)

# License

MIT
