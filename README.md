# Three Lakes System Decision Support System

Three Lakes System decision support system (TLS DSS) repository for forecasting water 
temperature in Shadow Mountain Reservoir using forecasted weather and varying 
operational pumping regimes.

The code in this repository is covered by the MIT use license. We request that 
all downstream uses of this work be available to the public when possible. During
development of the code in this repository, AI (Claude) was used to help create 
the architecture for the Streamlit App and was also used to help with coding the 
rollout of the forecast in Python, most code was modified in part or whole after 
creation by Claude through careful human review.

Repository contact: B Steele (b dot steele at colostate dot edu)

## Background

Water temperature is an indicator of water quality, as it governs much of the 
biological activity in freshwater systems. Northern Water, the municipal 
subdistrict that delivers drinking water to approximately 1 million people in 
northern Colorado and irrigation water for ~600,000 acres of land, has had 
recurring issues with water clarity in Grand Lake, the deepest natural lake in 
Colorado. They believe that the clarity issues in Grand Lake are primarily due 
to algal and diatom growth in Shadow Mountain reservoir which are pushed into 
Grand when they initiate pumping operations. Clarity in Grand is a focus of 
Northern Water, with the origin of that focus from 
Senate Document 80 which dates back to 1937 and the inception of the Colorado 
Big-Thompson project. In 2016 stakeholders and operators adopted a 
system of “goal qualifiers” for Grand Lake water Quality. The goal qualifiers are defined through 
Secchi disc depth measurements (a measure of water clarity), aiming for a 
3.8-meter Secchi depth average and 2.5-meter Secchi depth daily minimum to 
be met throughout the July 1 to September 11 Grand Lake Adaptive Management season.

Water in the Three Lakes System (TLS) naturally flows from Grand into Shadow Mountain 
into Granby, but pumping operations reverse that natural order by introducing 
hypolimnetic water (cold water) from Granby reservoir into Shadow Mountain.
This process reverses natural flow from Shadow Mountain into Grand and finally 
into the Alva B Adams tunnel to serve the Front Range (Figure 1). Northern 
suspects there is a biological “sweet spot” for water temperature in Shadow 
Mountain Reservoir that may reduce algal and diatom growth and therefore 
mitigate clarity impacts during pumping operations. Currently, Northern 
Water uses simulations of a computationally-intensive physical model to estimate 
clarity in Grand Lake; however these models take days to run and it is not 
possible to continually run them to create daily estimates, much less forecasts 
of either water temperature or clarity. This tool is meant to fill that gap.

![Figure 1](https://github.com/user-attachments/assets/1a22f221-96bb-4ed5-8911-25c08b40f501)

*Figure 1. Cartoon schematic of the three lakes system*

We have created an auto-regressive neural network to predict water temperature 
at two depth horizons (near surface 0-1m and integrated depth 0-5m) that 
incorporates many of the parameters of the physical model uses. 
This model is accurate and performs better than a persistence model 
(yesterday-is-today) and can make an estimate of temperature at the two depth 
horizons in a moment. The value of this model (and the decision support system) 
is the speed at which these estimates and forecasts can be made as well as the
accuracy. The usefulness of a decision support system is not just for this 
estimate of tomorrow’s temperature, but the ability to use forecasted 
meteorological and pump operations to estimate lake temperature days into the
future. This decision support system would allow for Northern Water and their
partners to test augmented pumping operations to determine the impacts to water
temperature (and therefore clarity), since we already know that the model is sensitive to changes in 
pumping operations. Currently, pumping operations are defined by expert
operators, meaning that operators have embedded knowledge of the system. The
hope is that between their expert knowledge and this data driven model, we can
provide additional context to the decisions these operators are making on daily 
basis.

## DSS Submodules

![Submodules](https://github.com/user-attachments/assets/a3318e7e-8146-4ca9-9e86-8f0da268058b)

*Figure 2. Sketch of decision support submodules for the TLS DSS*

## Repository Function

The data for this model is acquired using {targets} infrastructure. To run the workflow 
and update the underlying data, use the command `targets::tar_make()` in the R
console. 

## Python virtual environment

The Streamlit app's Python dependencies are pinned in 
[streamlit_app/requirements.txt](streamlit_app/requirements.txt) (joblib, matplotlib, 
numpy, pandas, streamlit, tensorflow, and scikit-learn).

The easiest way to run the app is with the `run_app.py` shortcut, which creates 
(or updates) a local `.venv_streamlit` virtual environment from that 
requirements file and then launches the app:

```
python run_app.py
```

If you'd rather manage the environment yourself, create a virtual environment 
and install from the same requirements file:

```
python -m venv .venv_streamlit
source .venv_streamlit/bin/activate
pip install -r streamlit_app/requirements.txt
streamlit run streamlit_app/app.py
```
