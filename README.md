# alx-backend-python
# Django-signals_orm-0x04
from django.db import models
from django.contrib.auth.models import User
from django.utils import timezone

class Message(models.Model):
    sender = models.ForeignKey(
        User, 
        on_delete=models.CASCADE,
        related_name='sent_messages'
    )
    receiver = models.ForeignKey(
        User, 
        on_delete=models.CASCADE,
        related_name='received_messages'
    )
    content = models.TextField(max_length=2000)
    timestamp = models.DateTimeField(default=timezone.now)
    is_read = models.BooleanField(default=False)
    
    class Meta:
        ordering = ['-timestamp']
        indexes = [
            models.Index(fields=['sender', 'receiver']),
            models.Index(fields=['timestamp']),
        ]
    
    def __str__(self):
        return f"{self.sender} to {self.receiver}: {self.content[:50]}"
