## updateEntity(value: { object })
Dispatching the createEntity function will send a `PATCH` to the backend, providing it a JSON API compliant object (not sure what this is? head to our "Good reads section").

```javascript
import { updateEntity } from 'redux-json-api'
class UpdateTask extends Component {
  constructor() {
    super();

    this.state = {
      taskListId: 1
    };
  }

  handleSubmit() {
    const { dispatch } = this.props;
    const entity = {
      id: '1',
      type: 'tasks',
      attributes: {
        task: 'New task string!'
      },
      relationships: {
        taskList: {
          data: {
            id: '1',
            type: 'taskLists'
          }
        }
      }
    }

    dispatch(updateEntity(entity));
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit.bind(this)}>
        <input type="text" onChange={this.handleChange.bind(this)}/>
        <button type="submit">Submit Task</button>
      </form>
    );
  }
}

export default connect()(UpdateTask);
```

When updating the task the `redux-json-api` will dispatch the following actions:

__API_WILL_UPDATE__
_This tells us what payload have been in the que for update, and what the `redux-json-api` will ship to the backend_
```javascript
action: {
  payload: {
    id: '1',
    type: 'tasks',
    attributes: {
      task: 'New task string!'
    },
    relationships: {
      taskList: {
        data: {
          id: '1',
          type: 'taskLists'
        }
      }
    }
  }
}
```

__API_UPDATED__
_This tells us the response object that we get back from the sever, where the backend have updated the single fields we provoided. Be aware that only the `task` key have updaterd and our completed remains the same._
```javascript
action: {
  payload: {
    id: '1',
    type: 'tasks',
    attributes: {
      task: 'New task string!',
      completed: false,
    },
    relationships: {
      taskList: {
        data: {
          id: '1',
          type: 'taskLists'
        }
      }
    }
  }
}
```