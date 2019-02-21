This project was bootstrapped with [Create React App](https://github.com/facebook/create-react-app).

## Handing Error
* Here we are going to create a global wrapper (HOC) to handle error globally.
* In this global error component we will show error message in model.
```js
import React, { Component } from 'react';

import Modal from '../../components/UI/Modal/Modal';
import Aux from '../Aux/Aux';

const withErrorHandler = ( WrappedComponent, axios ) => {
    return class extends Component {
        state = {
            error: null
        }

        componentWillMount () {
            axios.interceptors.request.use(req => {
                this.setState({error: null});
                return req;
            });
            axios.interceptors.response.use(res => res, error => {
                this.setState({error: error});
            });
        }

        errorConfirmedHandler = () => {
            this.setState({error: null});
        }

        render () {
            return (
                <Aux>
                    <Modal 
                        show={this.state.error}
                        modalClosed={this.errorConfirmedHandler}>
                        {this.state.error ? this.state.error.message : null}
                    </Modal>
                    <WrappedComponent {...this.props} />
                </Aux>
            );
        }
    }
}

export default withErrorHandler;

```
* in the above wrapper we are catching the common respose error with interceptor.
* here WrappedComponent is the actual component which using this wapper.ie when we are using this in Builder component before export we have to wrap with this withErrprHandler Hoc as follows

```js
import withErrorHandler from '../../hoc/withErrorHandler/withErrorHandler'
class BurgerBuilder extends Component {
    ...
}
export default withErrorHandler(BurgerBuilder,axios);
```















