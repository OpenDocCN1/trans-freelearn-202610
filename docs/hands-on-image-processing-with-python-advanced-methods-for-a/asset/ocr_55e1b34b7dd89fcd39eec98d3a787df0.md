Next is model wrapper. Let us define the following class that loads the restoration model and the language model, and integrates them:

```
```python
class InstructIR_Model:

    def __init__(self):
        seed_everything(SEED=SEED)
        torch.backends.cudnn.deterministic = True
        # Load config
        with open(os.path.join(CONFIG), "r") as f:
            config = yaml.safe_load(f)
        self.cfg = edict2namespace(config)
        # Create restoration model
        self.model = instructir.create_model(
            input_channels = self.cfg.model.in_ch,
            width = self.cfg.model.width,
            enc_blks = self.cfg.model.enc_blks,
            middle_blk_num = self.cfg.model.middle_blk_num,
            dec_blks = self.cfg.model.dec_blks,
            txtdim = self.cfg.model.txtdim).to(DEVICE)
        self.load_models()
```
```

This sets up the InstructIR network according to config and weights. Next step is model loading. Define the method load_models() to load both the image restoration model and text embedding head, as shown below:

```
```python
def load_models(self):
    # Load pretrained image restoration weights
    self.model.load_state_dict(torch.load(MODEL_NAME,
                                          map_location=DEVICE), strict=True)
    # Load language model for text instructions
    if self.cfg.model.use_text:
        self.language_model = LanguageModel(model=self.cfg.llm.model)
        self.lm_head = LMHead(embedding_dim=self.cfg.llm.model_dim,
                              hidden_dim=self.cfg.llm.embd_dim, num_classes=self.cfg.llm.nclasses)
        .to(DEVICE)
        self.lm_head.load_state_dict(torch.load(LM_MODEL,
                                                map_location=DEVICE), strict=True)
    else:
        self.language_model = self.lm_head = None
```
```

The function restore_img () in the next code snippet achieves the following:

+   - The input image is normalized and converted to a tensor.