# Kusi's Knowledge Base

## Get Picture from WebCam

### ISpFxWebCamProps.ts

```typescript
import * as Webcam from 'react-webcam';

export interface ISpFxWebCamProps {
    captureImage(img: string): void;
    close(): void;
}

export interface ISpFxWebCamState {
    imageData?: string;
    webcam?: Webcam;
    webcamopen?: boolean;
    facingMode?: boolean;
}
```

### SPFxWebCam.Module.scss

```scss
.spFxWebCam {
  position: fixed;
  top: 50px;
  left: 0;
  z-index: 1000;
  width: 100%;

  .container {
    max-width: 700px;
    margin: 0px auto;
    box-shadow: 0 2px 4px 0 rgba(0, 0, 0, 0.2), 0 25px 50px 0 rgba(0, 0, 0, 0.1);
    background-color: rgba(255, 255, 255, 0.7);
  }

  .title {
    background-color: #e7e7e7;
    padding: 10px;
    color: #000;
    font-family: Verdana;
    text-transform: uppercase;
    margin-bottom: 5px;
  }

  video {
    width: 100%;
    height: 100%;
  }

  .camContainer {
    padding: 20px;
  }

  .button:hover {
    background-color: #0076a8;
  }

  .button {
    // Our button
    text-decoration: none;
    height: 32px;
    margin-right: 10px;

    // Primary Button
    min-width: 80px;
    background-color: #009ee0;
    border-color: #009ee0;
    color: #ffffff;

    // Basic Button
    outline: transparent;
    position: relative;
    border-width: 0;
    text-align: center;
    cursor: pointer;
    display: inline-block;
    padding: 0 16px;

    .label {
      height: 32px;
      line-height: 32px;
      margin: 0 4px;
      vertical-align: top;
      display: inline-block;
      font-size: 14px;
      font-weight: bold;
    }
  }
}
```

### SpFxWebCam.tsx

```typescript
import * as React from 'react';
import * as Webcam from 'react-webcam';
import * as ReactDom from 'react-dom';
import styles from '../SpFxWebCam.module.scss';
import { ISpFxWebCamProps, ISpFxWebCamState } from './ISpFxWebCam';

export default class SpFxWebCam extends React.Component<ISpFxWebCamProps, ISpFxWebCamState> {
    private _camContainer: HTMLElement = undefined;

    public constructor(props: ISpFxWebCamProps) {
        super(props);
        this.state = {
            webcamopen: false,
            facingMode: false
        };
    }

    public render(): React.ReactElement<ISpFxWebCamProps> {
        return (
            <div>
                <div className={styles.spFxWebCam}>
                    <div className={styles.container}>
                        <h3 className={styles.title}>Title</h3>
                        {this.state.imageData
                            ? <span>
                                <a role='button' onClick={() => this.opencam()} className={styles.button}>
                                    <span className={styles.label}>New</span>
                                </a>
                                <a role='button' onClick={() => this.captureImage()} className={styles.button}>
                                    <span className={styles.label}>Save</span>
                                </a>
                            </span>
                            : !this.state.webcamopen
                                ? <a role='button' onClick={() => this.opencam()} className={styles.button}>
                                    <span className={styles.label}>{SchaltConstants.kameraOpen}</span>
                                </a>
                                : <span>
                                    <a role='button' onClick={() => this.capture()} className={styles.button}>
                                        <span className={styles.label}>Create</span>
                                    </a>
                                    <a role='button' onClick={() => this.changeCamera()} className={styles.button}>
                                        <span className={styles.label}>Change</span>
                                    </a>
                                </span>
                        }
                        <a role='button' onClick={() => this.close()} className={styles.button}>
                            <span className={styles.label}>Close</span>
                        </a>
                        <div id='camContainer' className={styles.camContainer}
                            ref={(elm) => { this._camContainer = elm; }}>
                        </div>
                        {this.state.imageData ? <div id='capturedPhoto' className={styles.camContainer}>
                            <img src={this.state.imageData}></img>
                        </div> : ''}
                    </div>
                </div>
            </div>
        );
    }

    private setRef = (webcam: Webcam) => {
        this.setState({ webcam: webcam });
    }
    private close(): void {
        ReactDom.unmountComponentAtNode(this._camContainer);
        this.setState({ webcamopen: false });
        this.props.close();
    }
    private capture(): void {
        const imageSrc: string = this.state.webcam.getScreenshot();
        ReactDom.unmountComponentAtNode(this._camContainer);
        this.setState({ webcamopen: false, imageData: imageSrc });
    }
    private captureImage(): void {
        this.props.captureImage(this.state.imageData);
        ReactDom.unmountComponentAtNode(this._camContainer);
        this.setState({ webcamopen: false });
        this.props.close();
    }
    private changeCamera(): void {
        ReactDom.unmountComponentAtNode(this._camContainer);
        this.setState({
            webcamopen: false,
            facingMode: !this.state.facingMode
        }, () => this.opencam());
    }
    private opencam(): void {
        const element: React.ReactElement<Webcam.WebcamProps> = React.createElement(
            Webcam,
            {
                height: 1280,
                width: 720,
                screenshotFormat: 'image/jpeg',
                videoConstraints: {
                    facingMode: { ideal: this.state && this.state.facingMode ? 'user' : 'environment' },
                    width: 720,
                    height: 1280,
                    aspectRatio: (720 / 1280)
                },
                ref: this.setRef
            }
        );
        ReactDom.render(element, this._camContainer);
        this.setState({ webcamopen: true, imageData: undefined });
    }
}
```