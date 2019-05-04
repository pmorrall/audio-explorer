# Audio Explorer

[Audio Explorer](http://audioexplorer.online) helps in audio data discovery and labelling by utilising unsupervised machine learning, statistics and digital signal processing.

The program computes audio features per short fragments of submitted audio piece and then finds projection to 2-dimensional space by using linear or [non-linear dimensionality reduction](https://en.wikipedia.org/wiki/Nonlinear_dimensionality_reduction). Audio fragments are then represented as points; similar sample will be close together, while those which are different further apart. User can click on a point to play the audio fragment and inspect resulting [spectrogram](https://en.wikipedia.org/wiki/Spectrogram). 

#### Why build it?

Manual labelling of audio is time consuming and error prone. With this tool we aim to augment user by allowing to easily navigate recordings and label selected audio pieces. Instead of looking at raw audio, we extract number of audio features from each sample. The latter typically consists of dozens of calculated values (features), which would be impossible to visualise (e.g. 20 features per sample effectively means 20-dimensional space). Audio Explorer allows to compute over 100 features per audio fragment.

The main driver behind creation of this software were problems I faced when developing an algorithm to classify bird calls for the [Royal Society for the Protection of Birds](https://www.rspb.org.uk/). 

#### How do we solve the problem?
We take the multidimensional space of computed audio features and project it to two dimensions, while retaining most of the information that describes the sample. That means that audio pieces that sound similar will be packed closely together, while those that sound quite different should be far apart. User can select cluster of similar-sounding samples and mark them.

## Gory details

#### Building blocks
The web application is made with Dash (Python + React) and is accompanied by a CLI (served through [click](https://click.palletsprojects.com/en/7.x/)). The web app is deployed with AWS Elastic Beanstalk and is supported by the following AWS services: EC2, S3, RDS, Secrets Manager, Route 53 and CloudWatch.

#### Behind the scenes

What's happening behind the scences when user hits upload: 
1. The audio file gets uploaded to the EC2 instance.
2. The file is converted to mono 16 bits per sample Waveform Audio File Format (WAV) and uploaded to S3. We'll be serving audio from signed S3 url.
3. Search for audio onsets according to supplied parameters. The onset detection can be disabled to process complete file.
4. Compute selected audio features per each audio fragment.
5. Run embedding algorithm over computed features and plot them. Each audio fragment becomes a point on the scatter plot that user can click to inspect spectrogram and play the audio.
6. Calculated audio features can be inspected, sorted and filtered through custom-made query language by selecting _Table_ tab.
7. User can now use e.g. _Lasso select_ (top right menu that appears after hovering over the graph) to select interesting cluster. The selection will be reflected in _Table_.
8. WIP: Clear noise with spectral subtraction. User will select noise and then run algorithm to remove undesired frequencies in a smart way. 



## Development

See [CONTRIBUTING](CONTRIBUTING.md).


## Acknowledgement

My thanks to:
* AWS Cloud Credits for Research for supporting the project! Thanks to AWS I could rapidly prototype the models and app itself.
* My colleagues from RSPB who have supplied the audio recordings and supported along the way.


## Licence

Audio Explorer is released under the version 3 of the GNU General Public License. Read COPYING for more details. The project is and will remain open source - that's a promise. 

## Contact information

```bash
python -c 'import base64; print(base64.b64decode("bHVrYXN6LnRyYWNld3NraUBvdXRsb29rLmNvbQ=="))'
```