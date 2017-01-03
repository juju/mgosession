# mgosession
--
    import "github.com/juju/mgosession"

Package mgosession provides multiplexing for MongoDB sessions. It is designed so
that many concurrent operations can be performed without using one MongoDB
socket connection for each operation.

## Usage

```go
var Clock clock.Clock = clock.WallClock
```

#### type Pool

```go
type Pool struct {
}
```

Pool represents a pool of mgo sessions.

#### func  NewPool

```go
func NewPool(ctx context.Context, s *mgo.Session, maxSessions int) *Pool
```
NewPool returns a session pool that maintains a maximum of maxSessions sessions
available for reuse.

#### func (*Pool) Close

```go
func (p *Pool) Close()
```
Close closes the pool. It may be called concurrently with other Pool methods,
but once called, a call to Session will panic.

#### func (*Pool) Reset

```go
func (p *Pool) Reset()
```
Reset resets the session pool so that no existing sessions will be reused. This
should be called when an unexpected error has been encountered using a session.

#### func (*Pool) Session

```go
func (p *Pool) Session(ctx context.Context) *mgo.Session
```
Session returns a new session from the pool. It may reuse an existing session
that has not been marked with DoNotReuse.

Session may be called concurrently.
